# Image Docker automatisant la creation d'un pool jdbc, la recuperation d'un war depuis bintray et son deploiement

## Installation
Récupérer les sources du projet
```
git https://github.com/remimomprive/dockerpayaracompose
cd dockerpayaracompose
```

Construire l'image de Payara
```
cd mypayara-full
docker build -t payara-config . \
--build-arg POSTGRES_PASSWORD=glassfishdbpassword \
--build-arg POSTGRES_USER=glassfish \
--build-arg POSTGRES_PORT=5432 \
--build-arg POSTGRES_HOST=db \
--build-arg POSTGRES_DATABASE=glassfish \
--build-arg POSTGRES_POOL_NAME=postgres-pool \
--build-arg APP_VERSION=1.0.0-devMax-9 \
--build-arg APP_SHORT_NAME=project-room-rest \
--build-arg PROJECT_DOWNLOAD_PATH=https://dl.bintray.com/maxrem/project-m2-room/fr/univtln/maxrem/projectroom
cd ..
```

Démarer le serveur d'application
```
docker-compose up -d
docker-compose logs -f
```

Accéder à l'application via : [http://localhost:8080/app](http://localhost:8080/app)

## Description du Dockerfile
Ce Dockerfile :
1. Créée le pool de connection JDBC à la base de données Postgres
2. Récupère le war de l'application
3. Déploie le war de l'application

## Description du docker-compose.yml
Le fichier docker-compose.yml démarre une base de données Postges et un serveur d'application Payara qui se connecte à la base de données et qui déploie le war.
