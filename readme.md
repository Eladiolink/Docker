## Docker

O docker é um sistema de virtualização não convencional, isso implica dizer que ao invés dele criar um máquina e simular ele, ele aproveita os recursos do sistema atual para rodar os containers.

## Containers

Os containers tratam de transportar nossa aplicação através de arquivos que facilitam e tornam mais prático, sem a necessidade de instalar um sistema completo, mas sim apenas o necessário.


A ideia de conteiner não é nova, o docker ele usava o principio de LXC (Linux Container).

Esse projeto usa por debaixo dos pandos diversas funcionalidade do Kernel Linux, tais como: chroot,cgroup, kernel namespace, kernel capabilities.

**Baixando imagens:**

```bash
docker pull <nome da imagem>
```

**Listando as imagens locais:**
```bash
docker images
```

**Executando Containers:**
```bash
docker run
ou
docker  run -i -t <nome da imagem>
```
O parametro **-i** indica que queremos um container interativo e o **-t** indica que queremos anexar o terminal virtual _tty_ do cotainer ao nosso host.

**Listando containers em execução:**
```bash
docker ps

ou

docker ps -a
```

Com o argumentop **-a** mostrará todos os container que já foram execultados na máquina host, sem necessariamente está em execução no momento.

**Removendo containers:**
```bash
docker rm <nome da imagem>
```

**Como são feitas as imagens?**

Nesse momento podemos pensar que o Docker é meio mágico (e é...kkk). Dado uma imagem ele pode rodar um ou mais containers com pouco esforço, mas como são feitas as images?

Uma imagem pode ser criada a partir de um arquivo de definição chamado de Dockerfile, nesse arquivo usamos algumas diretivas para declarar o que teremos na nossa imagem. Por exemplo se olharmos a definição da imagem do Ubuntu podemos ver algo semelhante a isso:

```dockerfile
FROM scratch ADD ubuntu-xenial-core-cloudimg-amd64-root.tar.gz / . . . RUN mkdir -p /run/systemd && echo 'docker' > /run/systemd/container

CMD \["/bin/bash"\]
```

## Comandos básicos

```bash
docker run <name_image>
```

O **docker run** verifica se tem a imagem local, caso não tenha ele baixa direto do Docker Hub ou futuramente a Docker Store. A imagem é as instruções para a criação de um container tipo uma receita de bolo para os nosso containers. E após baixada é iniciado o container. Com a flag *-d* cria o container e deixa ele em execução em backgorund. A flag *--name <nome>* é para vc informar ao docker um nome especifico para o container. A flat *-e* seta uma váriavel de ambiente ao container. A flag *-w "/var/www"* informa ao docker qual pasta ele irá executar os comandos.

```bash
docker ps
```

O **docker ps** lista todas os containers em execulção. E com a flag *-a*, mostra todos os container criados. Com a flag *-it* cria o container e entra no terminal atrelado ao container. A flag *-q* retorna apenas os id's dos containers

<br/>

Fica a dica ;) : docker stop $(docker ps -q)

```bash
docker start <nome_do_container>
docker stop <nome_do_container>
docker rm <nome_do_container>
docker container prune
```

O **docker start** inicia um container já iniciado e com a flag *-a* entra em um container com o terminal atrelado. E o **docker stop** finaliza um container em execução e com a flaf *-t 0* informa o tempo de espera para encerrar a execução. O **docker rm** remove um container. O **docker container prune** limpa todos os containers dodocker que não estão em execulção.

### Estados de um container

Running quando está rodando, Stopped quando está parado.

### Layered Filesystem

Cada imagem mais complexo, usa várias camadas, e por fim aproveita camadas já baixada localmente. E quando o Container é criado forma-se um *Read/Write Layer* para ser possivel fazer modificações e criação dentro do container já que a imagem e suas camadas é *Read Only*.

### Criando container com uma porta externa

```bash
docker run -P <nome_da_imagem>
docker run -p 4444:80 <nome_da_imagem>
docker port <id>
```

A flag *-P* informa ao docker que atribua uma porta do mundo externo ao container docker. com o *-p* é possivel informar uma porta dsejada para o docker mapear *sua_porta:porta_do_container*. O **docker port** informa qual porta o container está executando.


## Trabalhando com volumes no Docker

Volumes é uma apotação de uma pasta no container para uma pasta dentro do Docker Host.

```bash
docker run -v [local]:[caminho_do_container] <nome_da_imagem>
```

A flag *-v local:container* atrela  uma pasta local a uma determinada pasta em um container. E esse volume fica salvo no Docker Host ou seja no mesmo computador em que o Docker Engine estiver rodando.

## Criando Imagens

Cria um arquivo com *Dockerfile ou .dockerfile*.

```bash
FROM node:latest
ENV NODE_ENV=production
EN PORT=3000
MAINTAINER Eládio Leal
COPY ./Node /var/www
WORKDIR /var/www
RUN ["npm", "install"]
ENTRYPOINT npm start
EXPOSE $PORT
```

O **FROM** serve para informar qual imagem será usada como base nessa que questamos criando. O **ENV** serve para criar váriaveis de ambiente na imagem do container. O **MAINTAINER** Servirá para informar quem foi o criador da imagem. Com o **COPY** informaremos a pasta dos arquivos me desejamos copiar para a nossa imagem e onde ela ficará respectivamente. No **WORKDIR**  é usado para definir o diretório de trabalho de um contêiner do Docker a qualquer momento, ou seja onde rodará os comandos. Com o **RUN** é o comando que irá ser executado assim quando o container for criado. O  **ENTRYPOINT** servirá como um executor de comando por parâmetro. E o **EXPOSE** é para informar qual a porta que a aplicação usará no container (internamente).

```bash
docker build -f <arquivo> -t <nome_da_imagem> <onde_o_arquivo_está>
```

Para biuldar o dockerfile criado, basca execultar o comando acima com o _nome do arquivo_ (normalmente é Dockerfile ou tem a extensão .dockfile), _nome que a imagem terá_ e _onde o arquivo está_. Com isso será possível criar uma imagem docker localmente.

## Comunicação entre containers

Default network

```bash
docker network create --driver bridge minha-rede
```

```bash
 docker run -it --name ubuntu --network minha-rede ubuntu
```

## Docker Compose

### Execução

```bash
docker-compose build
```

Dá o comando para fazer bild do arquivo Yarm