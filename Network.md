# Comando docker network

As networks isolam os containers para que eles so se comuniquem com a rede que desejarmos.

### Para ver todos os comandos

```bash
    docker network --help
```

### Comandos create/ls

**create** - Serve para criar uma network

```bash
    docker network create <name>
```

```bash
    docker network create <name> -d <driver>
```

**ls** - Serve para listar os networks criados

```bash
```

````bash

    NETWORK ID      NAME        DRIVER      SCOPE
                                bridge
````

**bridge** - e uma network isolada que o docker cria, por padrao os containers que nao tem uma network especificada a utilizarao.

**null** - containers com essa network estao isolados e nao se comunicam com nenhum outro container.

### Conectando container a uma Network

```bash
    docker container run -d --name host --entrypoint /bin/sh --network <name> <image>
```

### Comandos inspect/rm

**inspect** - Inspecionar network

```bash
    docker network inspect <name>
```

**rm** - Remover uma network

```bash
    docker network rm <name>
```

### Comandos connect/disconnect

**connect** - Conecta um container a network

```bash
    docker network connect <name> <container>
```

**disconnect** - Serve para desconectar um container da rede.

```bash
    docker network disconnect <name> <container>
```