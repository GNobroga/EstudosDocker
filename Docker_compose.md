# Docker Compose

O Docker Compose é uma ferramenta que permite definir e gerenciar aplicativos multi-container com o Docker. Ele utiliza um arquivo YAML para descrever a configuração do ambiente e as dependências dos serviços que compõem o aplicativo.

### Comandos

```bash
    docker compose --help
```

### Comando up/down

**up** - Cria e inicia os containers

**down** - Pare e remove containers, networks.

### Arquivo DockerCompose

Criar arquivo: docker-compose.yml

```bash
    services:
        postgres:  # Esse nome pode ser o nome que eu quiser
            image: postgres
            command: # CMD
            container_name: postgres-test # Nome do container
            environment: 
            - POSTGRES_PASSWORD: postgres
            expose:
            - 5432
            restart: always
        
        wordpress:
            dependes_on: 
            - mysql
            image: wordpress
            restart: always

```

### Build no Service do Docker Compose

```bash
    services:
        frontend:
            image: name
            build: ./webapp # Vai buscar o dockerfile
        backend:
            image: name
            build:
                context: backend # Contexto
                dockerfile: ../backend.Dockerfile # Mencionar o dockerfile com nome diferente
                args:
                    GIT_COMMIT: cdsd5

```

### Volumes no Docker compose

```bash
    services:
        image: name
            volumes:
                - db-data:/etc/data:cached # Volume e para qual diretorio do container ele vai ser associado. :cached e uma opcao.
    
    volumes:
        db-data: # Nome dentro do docker file pra referenciar o volume
            name: "my-app-data"

```

### Network no Docker compose

```bash
    services 
        frontend:
            image: name
            networks
                - front-tier
                - back-tier
    networks:
        front-tier:
        back-tier:
            name: "bool"
            driver: bridge

```