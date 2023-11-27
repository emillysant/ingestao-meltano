## annotation

01 - pegar os arquivos docker-compose e data do nothwind

02 - criar a venv
    ```
    python3 -m venv venv
    ```

03 - ativar a venv
    ```
    source venv/bin/activate
    ```

04 - instalar meltano:
    ```
    pip install meltano
    meltano init 
    ```

        e adicionar o nome do projeto (pokedesk_meltano)

05 - apagar as pastas dentro do projeto 
    e deixar apenas .meltano e output

06 - entrar dentro do projeto meltano e roda
    ```
    cd pokedesk_meltano
    meltano install
    ```

07 - abrir outro terminal e levantar o container na root
    ```
    sudo docker compose up -d
    ```

08 - volta para pasta pokedesk_meltano
    ```    
    meltano add extractor tap-postgres
    ```

09 - instalar o plugin criado
    ```
    meltano install
    ```

obs: sempre rodar o load com extractor

10 - adicionar plugin de load e instalar 
    ```
    meltano add loader target-jsonl
    meltano install
    ```

11 - rodar o projeto
    ```
    meltano run tap-postgres target-jsonl
    ```

    11.1 - vai rodar um erro pq nao configurou a string conexao

    ```
    postgresql://[username]:[password]@localhost:5432/[db_name]
    ```

12 - pode apagar a pasta .meltano pra deletar as coisas q vc ja instalou
    e pode rodar o comando meltano install para criar novamente

13 - comando para listar arquivos do postgres

    ```
    meltano select tap-postgres --list
    ```

14 - pode se selecionar apenas uma tabela no banco
    adicione o comando dentro do arquivo meltano.yml
    ```    
    config: 
      sqlalchemy_url: postgresql://northwind_user:thewindisblowing@localhost:5432/northwind
    select: 
      - public-products.*
    ```

15 - rode novamente o projeto:
    
    ```
    meltano install
    meltano run tap-postgres target-jsonl
    ```

16 - observe o erro pois essa tabela tem dados diferentes ou faltando
    ```
    Failed validating 'type' in schema ['properties']
    ```

        16.1 - vc pode selecionar outra tabela

        ```    
        config: 
        sqlalchemy_url: postgresql://northwind_user:thewindisblowing@localhost:5432/northwind
        select: 
        - public-suppliers.*
        ```

## structure

```
tree -I 'venv|__pycache__|*.pyc|*.pyo|*.pyd'
```

├── data
│   ├── northwind.sql
│   └── warehouse.sql
├── dbdata  [error opening dir]
├── db_warehouse  [error opening dir]
├── docker-compose.yml
├── northwind_meltano
│   ├── meltano.yml
│   ├── output
│   │   └── public-suppliers.jsonl
│   ├── plugins
│   │   ├── extractors
│   │   │   ├── tap-postgres--meltanolabs.lock
│   │   │   └── tap-singer-jsonl--kgpayne.lock
│   │   └── loaders
│   │       ├── target-jsonl--andyh1203.lock
│   │       └── target-postgres--meltanolabs.lock
│   ├── README.md
│   └── requirements.txt
└── README.md

## Comando para carregar do data lake para arquivo json
```
meltano run tap-postgres target-jsonl
```

## Comando para carregar os arquivo json para o data warehouse
```
meltano run tap-singer-jsonl target-postgres
```

## plugins do meltano

https://hub.meltano.com/

# ingestao-meltano
