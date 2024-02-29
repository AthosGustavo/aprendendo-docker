# Aprendendo Docker

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
 
 
app.listen(process.env.PORT, ()=> {
  console.log('deu certo')
})
 
 FROM node:14
 WORKDIR /app-node
 ARG PORT_BUILD=6000
 ENV PORT=$PORT_BUILD
 EXPOSE $PORT_BUILD
 COPY . .
 RUN npm install
 ENTRYPOINT npm start
 
 docker build -t imagem-piloto/app-node:1.0 .
 docker images
 
 
 COPY . /app-node
  - Copia os arquivos do diretório atual para o diretório da imagem
  - Primeiro ponto: representa o diretório atual da máquina física, onde DockerFile está
 
 WORKDIR /app-node
 COPY ..
  - Primeiro ponto: representa o diretório atual da máquina física, onde DockerFile está
  - Segundo ponto: representa o diretório dentro da imagem
 
 
 
 
 
