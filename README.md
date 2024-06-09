# Aprendendo Docker
<details>
 <summary>Introdução</summary>

 # Introdução

 ## Virtualização
  - Se trata de uma máquina virtual isolada rodando em uma máquina física
  - Uma máquina física pode rodar mais de uma máquina virtual
  
 ## Container
  - Um Container não possui a necessidade de ter um sistema operacional
  - Para um container ser mantido em execução deve existir no mínimo um processo rodando dentro dele
  
 ### Princípio de isolamento NET
  - O Docker possui isolamento das interfaces de rede,a exemplo de portas
  - Isso quer dizer que se uma imagem estiver na porta 80, ao acessar a porta 80 no navegador, a aplicação não será acessada, pois é necessário mapear a porta do Docker para a porta da sua máquina.
 
 *Repositorios de imagens do Docker https://www.docker.com/products/docker-hub/*
  
 ### Comandos
  - Comando para baixar uma imagem: `docker pull nomeImagem`
  - Comando para rodar a imagem: `docker run nomeImagem`
    - Esse comando também serve para baixar e executar a imagem
  - Comando para baixar e rodar uma imagem: `docker run -d nomeImagem`
  
  - Comando para rodar o container e mapear a porta: `docker run -d -p 8080:80 nomeImagem`
    - 8080 representa a porta da máquina física
    - 80 representa a porta da imagem
    - -d roda o container sem travar o terminal
  
  - Comando para exibir os container em execução: `docker container ls`
  - Comando para exibir todos os containers que ja foram rodados: `docker container ls -a`
  
  - Comando para parar um container: `docker stop idContainer`
  - Comando para executar um container parado: `docker start idContainer`
  - Comando para pausar um container: `docker pause idContainer`
  - Comando para despausar um container: `docker unpauser idContainer`
  - Comando para remover um container: `docker rm idContainer`
  
  - Comando para adentrar no terminal de um container: `docker exec -it idContainer bash`
  
</details>


 
## docker-compose.yaml
Exemplo de configuração de um arquivo docker-compose.yaml para uma api em Spring boot e um banco de dados MySQL

```sql
version: '3.8'

services:
  mysql:
    container_name: mysql-container                       // nome do container
    image: mysql:8.0
    ports:
      - 3307:3306
    environment:
      - MYSQL_DATABASE=devops_uninassau
      - MYSQL_ROOT_PASSWORD=senhaRoot
      - MYSQL_USER=athos
      - MYSQL_PASSWORD=123
    networks:
      - devops-uninassau                                 // `network` representa a rede que este container fará parte

  java:
    container_name: java-container
    build:
      context: .
      dockerfile: ./docker/java/Dockerfile
    ports:
      - 8080:8080
    depends_on:
      - mysql
    volumes:
      - './src:/usr/app/src'
    expose:
      - '8080'
    networks:
      - devops-uninassau

networks:                                               // Criação da rede
  devops-uninassau:
    driver: bridge
```
-  Variáveis de ambiente necessárias para o funcionamento do mysql
   - `MYSQL_DATABASE=devops_uninassau`
   - `MYSQL_ROOT_PASSWORD=senhaRoot`
   - `MYSQL_USER=athos`
   - `MYSQL_PASSWORD=123`
   -  No entanto, apenas as variaveis MYSQL_DATABASE, MYSQL_ROOT_PASSWORD já é suficente para conexão
 
- `build` é a diretiva usada para construir um Dockerfile.`context` define o contexto de construção
 do arquivo,esse contexto envolve o acesso a arquivos do diretório especificado.
- `.` É usado para fazer
 referência ao diretório onde o arquivo docker-compose.yaml está localizado

<hr>

### Java

**Obsevação:** Note que não foi preciso declarar variáveis de ambiente do banco de dados na sessão do java.
- As informações de acesso ao MySQL são declaradas apenas no arquivo `application.properties`

#### Dockerfile java
```sql
FROM openjdk:17-jdk-alpine
WORKDIR /usr/app
COPY ../../target/java-correcao-docker-1.0.0.jar /usr/app/java-correcao-docker-1.0.0.jar

ENV DOCKERIZE_VERSION v0.7.0
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz

EXPOSE 8080

CMD dockerize -wait tcp://mysql:3306 -timeout 60s java -jar java-correcao-docker-1.0.0.jar
```

