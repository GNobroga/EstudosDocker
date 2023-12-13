# Conceito de Tags em Imagens

**Tag** - São versões da uma imagem

**--help** - Para exibir subcomandos, quase todos os subcomandos do docker possuem essa flag.

# Comando docker image

### Ver os comandos do subcomando image:
<br>

```bash
    docker image --help
```
### Baixar imagem do repositório remoto:
<br>

```bash
    docker image pull <image:version>
```

### Listar as imagens presentes no host
<br>

```bash
    docker image ls
```

### Inspecionar uma imagem (Informacoes)
<br>

```bash
    docker image inspect
```
### Salvar uma imagem em um arquivo compactado
<br>

```bash
    docker image save <image> -o path/filename.tar
```

**-o** - Essa flag informa o output que sera salvo a imagem compactada.

### Remover imagem dentro do host
<br>

```bash
    docker image rm <image>
```

É possível remover a imagem pelo hash ou pelo nome quando possível.

### Carregar uma imagem compactada 
<br>

```bash
    docker image load -i path/filename.tar
```

**-i** - Significa a entrada dos dados

### Importar uma imagem
<br>


**import** Esse comando importa uma imagem baseada em um container.

```bash
    docker image import <fileName.tar> <nameImage>
```
