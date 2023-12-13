# Informacoes do Sistema 

````bash
    docker system info
````

# Conceito de containers

Os containers são formados por uma imagem.

**--help** - Para exibir subcomandos, quase todos os subcomandos do docker possuem essa flag.

### Ver os containers subcomandos
<br>

````bash
    docker container --help
````

### Ver os containers em execucao
<br>

````bash
    docker container ls
````
**-a** - Para ver todos os containers e mais detalhes

### Rodar uma container
<br>

````bash
    docker container run <image>
````

Dentro do Dockerfile de uma imagem pode existir um **ENTRYPOINT** e um **CMD** e é possível alterar esses comandos ao executar o **docker container run**.

O **ENTRYPOINT** especifica o comando ou script que será executado quando o contêiner for iniciado.

O **CMD** é usado para fornecer argumentos padrão para o ENTRYPOINT ou para especificar um comando a ser executado se nenhum ENTRYPOINT estiver definido.

**Exemplo para alterar o CMD**: 
<br>

````bash
    docker container run <image> command 
````

**Exemplo para alterar o Entrypoint**: 
<br>

````bash
    docker container run --entrypoint /bin/bash <image>
````

### Rodar um container em segundo plano ou de forma iterativo
<br>

**Rodar em segundo plano sem travar o terminal**: 

<br>

````bash
    docker container run -d <image>
````

<br>

**Rodar de forma iterativa**: 

<br>

````bash
    docker container run -i <image> --entrypoint /bin/bash
````

Colocando o entrypoint /bin/bash eu altero o Dockerfile da image para ao executar
ter um terminal bash associado.

**Rodar de forma iterativa e associar um terminal**: 

<br>

````bash
    docker container run -it <image> --entrypoint /bin/bash
````

**-i** - Executa o container em modo iterativo, ou seja, permite com que o container interaja com o usuario.

**-t** - Associa uma terminal ao container para interecao

### Rodar o container e fazer com que ele seja removido quando parar sua execucao.

<br>

````bash
    docker container run <image> --rm
````

### Dar um nome customizado para o container

<br>

````bash
    docker container run --name <name> <image> 
````

### Alguns comandos create/start/stop/kill/pause/unpause/rm

<br>

**create** - Esse comando cria um container

````bash
    docker container create --name <name> <image>
````

**start** - Esse comando executa um container

````bash
    docker container start <name>
````

**stop** - Esse comando fala para o container para sua execucao quando puder.

````bash
    docker container stop <name>
````

**kill** - Esse comando forca o container a para sua execucao.

````bash
    docker container kill <name>
````

**pause** - Ele paralizar o container, ele continua em execucao.

````bash
    docker container pause <name>
````

**unpause** - Ele  desparaliz o container.

````bash
    docker container unpause <name>
````

**rm** - Remove um container que nao esteja rodando.

````bash
    docker container rm <name>
````

**--force** - Forca uma remocao, caso o container esteja em execucao.

### Comandos restart/rename/wait

**restart** - Serve para reiniciar um container

````bash
    docker container restart <name>
````

**rename** - Serve para alterar o nome do container

````bash
    docker container rename <oldName> <newName>
````

**wait** - Deixa a linha de comando presa ate que o container pare sua execucao, assim que ele parar sera exibido o **exit code**.

````bash
    docker container wait <name>
````

### Associar uma porta do host ao container


````bash
    docker container run --name <name> image -p <hostPort>:<containerPort>
````

Para associar uma porta é necessário verificar o Dockerfile da imagem para ver 
qual porta ela **expoem** para o host.


### Comandos inspect/port/diff/logs

**inspect** - Exibe informacoes relacionadas ao container

````bash
    docker container inspect <name>
````

**port** - Serve para listar as mapeadas do host para o container

````bash
    docker container port <name>
````

**diff** - Serve para verificar se houve alguma mudanca no container baseada na imagem que foi criado, ou seja, se um arquivo for criado dentro do container ele exibe isso pra gente.

````bash
    docker container diff <name>
````

**logs** - Serve para exibir logs do container quando algo acontece.

````bash
    docker container logs <name>
````

**-f** - Deixa a linha de comando presa para exibir os logs.

### Comandos exec/cp

**exec** - Serve para executar comandos dentro do container ou entrar dentro do container para interagir.

````bash
    docker container exec -it <name> <shell(bash/sh)>
````

````bash
    docker container exec <name> <command>
````

**cp** - Serve para copiar arquivos de dentro do container para o host.

````bash
    docker container cp <name>:<originPath> <destPath>
````

### Variaveis de Ambiente, Hostname e Read Only

**-e** - Flag para indicar uma variavel de ambiente.

````bash
    docker container run --name <name> <image> -e VAR1=value -e VAR2=value
````

Eu posso tanto criar uma variavel, quanto substituir o valor de uma existente
com essa flag. Eu consigo ver essas variavies no inspect do container.

**--read-only** - Flag para indicar que o container e apenas para leitura, ou seja, ele nao podera sofrer modificacoes internas, como adicionar arquivos, etc.


````bash
    docker container run --name <name> <image> --read-only
````

**--hostname ou -h** - Serve para definir o host dentro do container, ou seja,
permite costumizar o nome do container ao inves de usar um nome gerado pelo proprio Docker.

````bash
    docker container run --name <name> <image> --hostname <name>
````

### Verificar se o container esta saudavel (Health Check)

Isso valida se o container esta funcionando corretamente baseado no comando de teste.

**--health-cmd** - Serve para executar o comando de validacao de saude, se o comando executar corretamente significa que o container passou no teste e esta saudavel (funcionando  de acordo com o esperado)

**--health-interval** - Seta um intervalo para ficar validando se o container esta saudavel.

**--health-retries** - Seta a quantidade maxima de validacao de saude que sera feita no container.

**--health-start-period** - Seta um intervalo para executar o comando de validacao depois do valor espeficificado.

````bash
    docker container run --name <name> --health-cmd "cat /usr/share/nginx/html/index.html" --health-interval 500ms --health-retries 3 --health-start-period 600ms <image>
````

### Comando commit

**commit** - Serve para criar uma nova imagem baseada no estado atual do nosso container.

````bash
    docker container commit <name> <imageNameIWant>
````

**-p** - Pausa o container naquele momento para fazer o commit e depois que finalizar retorna.

### Comando export

**export** - Salvo o container em seu estado atual para um arquivo compactado.

````bash
    docker container export <name> -o <fileName.tar>
````

### Limitar recursos de um container

**--help** - Para ver as flags que podem ser usadas no run

**--cpus** - Serve para limitar o uso de CPU

**-m** - Serve para limitar o uso de memoria

````bash
    docker container run --name <name> --cpus 0.1 -m 100m <image>
````

### Comandos stats/top/update

**stats** - Serve para pegar em tempo real o consumo de recursos de um container, uso de memoria, cpu, etc.

````bash
    docker container stats <name>
````

**top** - Serve para verificar os processos em execucao dentro de um container

````bash
    docker container top <name>
````

**update** - Serve para atualizar o uso de recursos, por exemplo, eu quero adicionar mais memoria, mas cpu pro container.

````bash
    docker container update <name> --cpus 0.8
````

### Flags do Subcomandos do docker container run (Sub) - pull/restart/privileged

**--pull** - Permite definir se ao criar o container o docker vai baixar a imagem ou nao, por padrao e (missing) o que significa que so baixa caso nao exista. (missing, always, never)

````bash
    docker container run --name <name> --pull always <image>
````
**--restart** - Define se o container vai ser reiniciado em algumas ocasioes especificadas. Por exemplo, ao ligar o computador o container pode ser executado automaticamente ou quando finaliza seu ciclo ele pode reiniciar, o padrao e (no). (no, always, on-failure, unless-stopped)

````bash
    docker container run --name <name> --restart always <image>
````

***--privileged** - Serve para dar permissoes especiais ao container para executar algum determinado comando que exija isso. Exemplo criar um novo disco, etc.

````bash
    docker container run --name <name> --privileged <image>
````

#### Sub comandos do docker system

**df** - Serve para visualizar o espaco em disco que esta sendo usado pelos objetos (Images, Containers, Volumes, etc) do docker.

````bash
    docker system df
````

**events** - Vai mostrar todas as coisas que estao acontencendo dentro do docker, como se fosse um registro de logs.

````bash
    docker system events
````