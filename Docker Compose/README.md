# Docker Composer

Um arquivo Docker-Composer é dividido em três partes básicas sendo elas: version, services, volumes e networks. Mas dendendendo também da necessidade do uso.

### Version 

É usado para definir a versão do docker composer que será usado, no exemplo a seguir é indicado que a versão será usada é a 3.4:
```yml
version: 3.4
```

### Services

Como já dizemos para o docker composer qual será a versão usada, vamos definir para o docker os containers na qual queremos que rode por meio de informar os serviços que serão executados. 

#### build

O **build** serve para informar onde está localizado o arquivo para a qual o docker utilizará na contrução da imagem do serviço em específico.
```yml
build: ./dir
```
Nesse exemplo o arquivo docker file está dentro da pastar *dir*. Porém pode ser informado o lugar específico e o nome do arquivo utilizando o *context* e o *dockerfile*, que servirá para informar a pasta e o nome do arquivo respectivamente, observe o exemplo:
```yml
build:
      context: ./dir
      dockerfile: imagem.dockerfile
```

#### image

Após definir o *build* é ideal informar qual será o nome dessa imagem gerada para o serviço, isso pode ser feito de maneira simples com o **image**, e apenas definir o nome da imagem, segue o exemplo:
```yml
image: exemplo/service
```
Então para esse serviço o docker criará a imagem com o nome *exemplo/service*.

####  container_name

Agora é importante falarmos para o docker compose qual deve ser o nome do nosso container no qual ele usará para esse serviço, e isso é bem simple atráves do *container_name* como podemos observar no exemplo:
```yml
 container_name: exemplo-service
```
Com essa vreve instrução o nome do nosso container será apenas exemplo-service.

#### ports
Serve para informar ao docker composer quais portas serão expostas na hora da sua criação, analogo a flag *-p* na criação de uma container.
```yml
ports:
    - "80:80"
```
Nesse breve exemplo, dizemos para o docker composer que queremos que ele faça uma ligação da porta 80 da nossa conexão externa ao container, com a porta 80 dentro nosso container também.

#### networks
Usamos para informar para o docker composer qual será o driver de conexão de rede no qual o nosso serviço irá utiliza, e é importante caso queiramos deixar claro para o nosso service que ele utilizará um drive customizado criado por nós.
```yml
networks:
      - production-network
```
Nesse exemplo acima, é informado para o docker composer que o driverd de rede, é um customizado chamado *production-network*, mas nada impede de também usar o padrões do docker como: host, bridge, none.

#### depends_on
Caso desejamos informar para o nosso docker composer que o serviço irá depender de outros serviços de pé para ser levantado, então usamos o *depends_on*.
```yml
depends_on:
    - service1
    - service2
    - service3
```
Nesse exemplo é informado ao docker composer, que esse nosso serviço depende do: service1, service2, service3, portando ele só ficará de pé caso esses três serviços já estejam ativos.

Agora juntando todos esses exemplo citados, temos que nosso serviço ficará da seguinte forma:
```yml
services:
   service-exemple:
    build:
      context: ./dir
      dockerfile: imagem.dockerfile
    image: exemplo/service
    container_name: exemplo-service
    ports:
        - "80:80"
    networks:
        - production-network
    depends_on:
        - service1
        - service2
        - service3
```

Dentro do escopo de **services** pode ser definidas vários servições, porém tudo depende da necessidade ok? ;)