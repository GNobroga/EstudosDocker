# Conceito de Camadas em Imagens

Uma imagem Docker é composta por uma série de camadas, onde cada camada representa uma alteração incremental em relação à camada anterior. Essas camadas são empilhadas para formar a imagem completa.

Ao criar um container, o Docker Engine adiciona uma camada de
**Read/Write** no topo das camadas da imagem.

Todas as alteracoes que forem feitas enquanto o container existir
acontencerao nessa camada.

Quando o container e removido essa camada tambem sera, deixando
as camadas da imagem intactas.

# Comandos de um Dockerfile

**FROM** 

**RUN** 

**CMD** 

**LABEL** 

**EXPOSE** 

**ENV** 

**ADD** 

**COPY** 

**ENTRYPOINT** 

**VOLUME** 

**USER** 

**ARG**

**WORKDIR**

**ONBUILD**

**STOPSIGNAL**

**HEALTHCHECK**

**SHELL**

**DENTRE OUTROS**

# Comandos que produzem camadas na imagem

**FROM**

**ADD**

**COPY**

**RUN**

Essas camadas nao se alteram quando a imagem e usado para criar um container (sao imutaveis, ou seja, sao READ-ONLY).

# Visualizando Camadas de Imagens e Containers no Host

Isso nao e tao necessario, mas vou por para ter mais detalhes.

**O local que fica as camadas no Linux**:

Dir: cd /var/lib/docker/overlay2

**ou**

````bash
    docker image inspect <name>
````

# Estrutura Basica do Dockerfile

**Nome para o arquivo**: Dockerfile 

Não é obrigatório escrever o nome das instruções no Dockerfile em maiúsculo, porém, o Docker recomenda que se faça isso.

**Exemplo**:

````bash
    # Isto e um comentario
    ADD arguments
````

Um Dockerfile atualmente pode ter 17 possiveis instruções. Além disso, o arquivo Dockerfile deve sempre começar com a instrução **FROM** ou em situações específicas com o **ARG**. Apenas com esses dois.

````bash
    # Isto e um comentario
````

# Instruções FROM/ARG/ENV/LABEL

**FROM** - Serve para especificar uma imagem base.

**ARG** - Serve para configurar variáveis durante o build do Dockerfile. Além disso, é possível definir valores padrões para caso não seja passado o valor no build para a variável. Para utilizar essa variável no próprio arquivo do Dockerfile usa-se **$NOME**. 

**ENV** - Serve para configurar variáveis de ambientes que vão estar presentes na imagem final. Não é possível configurar na hora do build do Dockerfile.

**LABEL** - Serve para documentar o que eu estou fazendo.

````bash
    ARG TAG=latest # Exemplo de quando se usa ARG no começo do Dockerfile

    FROM  <image>$TAG

    ARG CONTAINER_NAME
    ARG CONTANER_NAME2=Container2

    ENV USERNAME=$CONTANER_NAME2

    ENV USERNAME2=GabrielUsername2 

    ENV PASSWORD=${CONTAINER_NAME2:-Gabriel} # Caso a variavel CONTAINER_NAME2 nao estiver com um valor setado vai definir Gabriel como padrao.

    LABEL maintainer="Gabriel Cardoso Girarde"
````

# Comando Docker image build

**.** - Vai buscar o arquivo Dockerfile
**-t** - Para definir um nome e uma tag opcionalmente.

````bash
    docker image build . -f <DockerfilePath> -t <name> # <name>:<tag>
````

**OU**

````bash
    docker image build .
````

**OU**

````bash
    docker image build -t <name> <currentDir>
````
**OU**

````bash
    docker image build -t <name> --build-arg ARG1=value  --build-arg ARG2="oi" .
````
# Instrucoes COPY/ADD/WORKDIR/RUN

**WORKDIR** - Serve para alterar ou criar um diretório no container, os comandos que vierem abaixo dessa instrução serão executados dentro desse diretório.

**COPY** - Serve para copiar um arquivo do host para dentro da nova imagem que será criada.

**ADD** - Também serve para copiar arquivos do host para a imagem, mas também permite pegar arquivos da URL para dentro da imagem, além de copiar arquivos compactados e descompactar automaticamento dentro atual workdir da imagem. (Só utilizar para copiar arquivos da URL ou arquivos compactados, pois eles serão automaticamente descompactado)

**RUN** - Serve para executar comandos dentro da imagem para deixá-la do jeito que queremos.

````bash
    FROM ubuntu:18.04

    WORKDIR /copy # Vai criar o diretorio copy dentro do container

    COPY namefile . # . ou /copy

    WORKDIR /add

    ADD filename.tar . # . ou /add
    ADD www.google.com.br/image?id=1 . # . ou /add

    RUN apt-get update && apt-get install -y \ # \ - Serve pra quebrar linha
        apt-get install node -y
````

# Instrucoes ENTRYPOINT/CMD

**ENTRYPOINT** -  É usado para especificar o executável ou o comando que será executado quando o contêiner for iniciado. Ele define o ponto de entrada principal para o contêiner e normalmente contém o executável principal ou o script que deve ser executado quando o contêiner é iniciado. O ENTRYPOINT também permite que você passe argumentos para o executável ou script especificado.

**CMD** - Já o CMD é usado para fornecer argumentos padrão para o comando definido no ENTRYPOINT, ou como comando padrão caso o ENTRYPOINT não esteja definido. A instrução CMD pode ser substituída por argumentos fornecidos durante a execução do contêiner. Se o ENTRYPOINT for definido, os argumentos do CMD serão passados para ele. Caso contrário, o CMD será executado como o comando principal.

A diferença entre CMD e ENTRYPOINT para o **RUN** é que o RUN só executa os comandos em fase de construção da imagem, enquanto o CMD ou ENTRYPOINT são executados quando o container é criado a partir da imagem específicada.

**Formato EXEC** - É representando com [] e é recomendado pelo Docker para executar comandos no ENTRYPOINT OU NO CMD.


````bash
    FROM alpine:3.16.2

    RUN apk add ansible

    ENTRYPOINT ["ansible"]

    CMD ["--help"]

    # ansible --help
````

# Instrucoes EXPOSE/VOLUME/USER

**EXPOSE** - Serve para definir um porta que será exposta ao container.

**VOLUME** - Serve para definir um volume que será automaticamente criado quando rodamos o container. Os volumes no Docker são usados para armazenar dados que precisam ser persistentes e acessíveis entre várias execuções de contêineres. Eles são usados para compartilhar arquivos ou diretórios com o host ou outros contêineres. Comando:  **docker volume ls**

**USER** - Serve para definirmos um novo usuário para rodar os comandos do Dockefile. Ao definir o USER, você pode especificar um usuário diferente do usuário padrão (normalmente root) para executar os comandos subsequentes. Isso ajuda a aumentar a segurança, já que executar processos dentro do contêiner com um usuário não privilegiado reduz o risco de acesso não autorizado a recursos do sistema caso ocorram violações de segurança.

````bash
    FROM nginx:latest

    COPY filename /etc/nginx/conf.d/default.conf
    COPY nginx.conf /etc/nginx/nginx.conf

    USER gabriel # Gabriel ou Gabriel:Grupo

    EXPOSE 8010

    VOLUME /usr/share/nginx/html

````
# Instrucao SHELL

**SHELL** -  A instrução SHELL no Dockerfile é usada para definir o shell padrão que será usado para interpretar os comandos subsequentes no Dockerfile.

````bash
    FROM ubuntu

    SHELL ["/bin/bash", "-c"]

    RUN touch file-{0..10}
````

# Instrucoes STOPSIGNAL/HEALTHCHECK

**STOPSIGNAL** - Serve para configurar o sinal que o container vai emitir quando for interrompido.

**HEALTHCHECK** - Serve para executar um comando para verificar se o container funciona corretamente. O comando CMD no contexto do HEALTHCHECK e usado para
executar o comando que verificara a saude do container.

````bash
    FROM nginx

    HEALTHCHECK --interval=10s --timeout=5s --start-period=15s --retries=2 \
        CMD curl -f http://localhost/

    STOPSIGNAL SIGKILL
````

# Instrucoes ONBUILD

**ONBUILD** - Serve para definir instruções para imagens que são criadas a partir dessa imagem, não é possível colocar instruções como o próprio ONBUILD e FROM. 

````bash
    FROM nginx 

    ONBUILD WORKDIR /var/www
    ONBUILD COPY index.html .
    ONBUILD RUN echo "Eu nao sou o goku" > index.html
````

````bash
    docker image build -t nginx:onbuild -f Dockefile 
````

````bash
    FROM nginx:onbuild # imagem que ja existe
````

# Comando docker login e logout

O comando docker login é usado para autenticar-se em um registro de contêiner, como o Docker Hub, para ter acesso a imagens privadas ou para fazer o upload de imagens personalizadas para um registro. Ao executar o comando docker login, você será solicitado a fornecer suas credenciais de autenticação, incluindo um nome de usuário e uma senha ou um token de acesso. Essas credenciais serão usadas para verificar sua identidade e conceder acesso aos recursos do registro de contêiner.

````bash
    docker login -u username
````

**ou** 

````bash
    docker login -u username -p <token>
````

# Sub comandos do docker image tag/push/history

**tag** - Serve para alterar a tag de uma imagem.

````bash
    docker image tag <name> <newName>
````

**push** - Serve para fazer o upload da imagem para o container registry ( docker hub, gitlab, aws, etc).

````bash
    docker image push <image>
````
**history** - é usado para exibir o histórico de camadas de uma imagem Docker. Ele mostra informações sobre cada camada presente na imagem, incluindo o ID da camada, o comando executado para criar a camada e o tamanho da camada.

````bash
    docker image history <image>
````

# Sub comando do docker container attach

**attach** - Serve para associar o nosso terminal  a um terminal de um container que em execucao. Pra usar o attach é necessario executar o container de forma interativa e em segundo plano.

````bash
    docker container run -d -it --rm --name python python:attach
````

````bash
    docker container attach <name>
````

**detach-keys** - Flag para definir o comando pra sair do container.

# Uso do cache no build

Isso pode acelerar a criação de uma nova imagem, por padrao isso já é utilizado pelo Docker. 

**--no-cache** - Flag pra indicar no build que não é pra usar o cache.
**--cache-from** - A flag --cache-from é usada no comando docker build para indicar uma imagem base existente a partir da qual o Docker pode aproveitar o cache durante o processo de construção da imagem. Isso pode acelerar o tempo de construção, evitando a necessidade de reconstruir camadas já presentes na imagem base.

````bash
    docker image build -t <name> -f Dockerfile --no-cache
````

# Build com Múltiplas Estágios

O build com múltiplos estágios (multi-stage build) é uma técnica do Docker que permite criar uma imagem Docker de forma mais eficiente, reduzindo o tamanho final da imagem e evitando a inclusão de arquivos e dependências desnecessárias.

````bash
    # Estágio 1: Compilar e gerar os arquivos estáticos
    FROM node:alpine as builder
    WORKDIR /app
    COPY package.json .
    RUN npm install
    COPY . .
    RUN npm run build

    # Estágio 2: Imagem final
    FROM nginx:alpine
    COPY --from=builder /app/build /usr/share/nginx/html

````

# Arquivo .dockerignore

O COPY dessa imagem abaixo vai copiar todos os arquivos do diretorio do host para o container criado.

````bash
    FROM alpine:3.17

    WORKDIR /app

    COPY . .
````

**.dockerignore** - Arquivo para excluir alguns arquivos para que nao seja copiado no COPY para o container.