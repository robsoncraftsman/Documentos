# Dicas Docker

## Instalação Linux

https://docs.docker.com/engine/install/ubuntu/

## Comandos Docker

### Administração de imagens

- Ver todas as imagens instaladas:

`docker image ls`

- Baixar uma imagem do repositório:

*Importante:Definir a versão da imagem. Por padrão é baixada o latest. Mas isso pode gerar problemas de compatibilidade/segurança no futuro*

`docker image pull {image_name}:{image_version}`

- Remover uma imagem local:

`docker image rm {image_name|id}`

- Inspecionar a imagem:

`docker image inspect {image_name|id}`

- Criar um alias/versão para a imagem:

`docker image tag {image_name|id}:{version} {alias}:{version}`

- Criar uma nova imagem a partir de um descritor:

`docker image build {params}`

- Enviar uma imagem para o repostitório:

`docker image push {params}`

### Administração de containers

- Ver os containers docker rodando:

`docker container ls`

- Ver todos containers, inclusive os parados:

`docker container ls -a`

- Baixar, instalar e rodar uma imagem:
*- Importante(1): definir um nome, caso contrário, a cada comando run, um novo container é criado na sua máquina*
*- Importante(2): rodar o container em modo background para serviços*
*Importante(3):Definir a versão da imagem. Por padrão é baixada o latest. Mas isso pode gerar problemas de compatibilidade/segurança no futuro*

`docker container run -d --name {name} {params} {image_name}:{image_version}`

- Parar uma imagem:

`docker container stop {container_name|id}`

- Iniciar uma imagem:

`docker container start {container_name|id}`

- Reinicar uma imagem:

`docker container restart {container_name|id}`

- Excluir uma imagem:

`docker container rm {container_name|id}`

### Integração container/SO

- Mapear uma porta para acesso externo:

`docker container run -p {porta_externa}:{porta_interna} {params}`

- Mapear uma pasta do SO para ser acessada pelo container:

`docker container run -v {pasta_so}:{pasta_container}`

### Logs/Config do container

- Visualizar os logs de um container que está rodando em background:

`docker container logs {container_name|id}`

- Visualizar as configurações de um container:

`docker container inspect {container_name|id}`

### Executar comandos no container

- Entrar na linha de comando do container:

`docker container exec -it {container_name|id} bash`

- Verificar a versão do SO do container:

`docker container exec {container_name|id} cat /etc/os-release`

### Build de imagens

- Gerar uma nova imagem a partir da pasta corrente (onde está o arquivo Dockerfile):

`docker image build -t {image_name} .`

### Subir imagem para o DockerHub

- Logar no DockerHub

`docker login --username={user}`

- Enviar uma imagem:

`docker image push {image_name|id}`

### Redes do Docker

Modo padrão do Docker é "Bridge".

- Listar as redes do Docker:

`docker network ls`

- Para criar uma rede específica:

`docker network create --driver bridge {network_name}`

- Para inspecionar uma rede (ver seus atributos/ip):

`docker network inspect {network_name}` 

- Para permitir que um container enxergue outra rede:

`docker network connect {params} {network_name} {container_name}`

- Para selecionar a rede a ser utilizada pelo container

`docker run --net {none|bridge|host} {params}`

## Exemplos

### POSTGRES

```
docker run \
 --name postgres \
 -e POSTGRES_USER=admin \
 -e POSTGRES_PASSWORD=pwd \
 -e POSTGRES_DB=herois \
 -p 5432:5432 \
 -d \
 postgres
```
```
docker run \
 --name adminer \
 -p 8001:8080 \
 --link postgres:postgres \
 -d \
 adminer
```

### MONGODB

```
docker run \
 --name mongodb \
 -p 27217:27017 \
 -e MONGO_INITDB_ROOT_USERNAME=admin \
 -e MONGO_INITDB_ROOT_PASSWORD=pwd \
 -d \
 mongo:4
```
```
docker run \
 --name mongoclient \
 -p 8002:3000 \
 --link mongodb:mongodb \
 -d \
 mongoclient/mongoclient
```
```
docker exec -it mongodb \
 mongo --host localhost --port 27017 -u admin -p pwd --authenticationDatabase admin \
 --eval "db.getSiblingDB('herois').createUser({user: 'user', pwd: 'pwd', roles: [{role: 'readWrite', db: 'herois'}]})"
```
