# Comando docker volume

### Comando para ver os subcomandos do comando volume

O volume serve para persistir dados de um container, ou seja, nao ha perdas quando o container for deletado.

```bash
    docker volume --help
```

### Comandos create/ls

**create** - Cria um volume

````bash
    docker volume create <name> # o nome e Opcional
````

````bash
    docker volume create --name <name> # o nome e Opcional
````

**ls** - Exibe os volumes

````bash
    docker volume ls
````


### Associando um Volume ao container

```bash
    docker container run --name <name> -it --rm --volume <volume>:<dir> <image>
```
dir - Diretorio dentro do container a ser persistido.


### Comandos inspect/rm

**inspect** - Serve para inspecionar 

```bash
    docker volume inspect <name>
```

**rm** - Serve para remover

```bash
    docker volume rm <name>
```

### Associando um diretorio do host ao container (Bind mount)

Isso serve para associar um diretorio do nosso sistema de arquivos em um container.

```bash
    docker container run -it --rm --name <name> -v <dirSystemFile>:<dirContainer>
```
