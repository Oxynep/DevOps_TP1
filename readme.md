https://drive.google.com/drive/folders/13-1rq6Fkwb27RnhEK9vztQWLXxxY4l5t

dans tout le dépot "front" fait référence au proxy; le front est nommé "front_main"

## first step
newgrp docker<br>
ssh -T git@github.com

# DATABASE
#### Build
```
docker build -t oxynep/cours/tp1_db .
```

## Run
```
docker network create net
docker run -d --name db_serv --network net oxynep/cours/tp1_db
docker run -d --name db_view --network net -p 8080:8080 adminer
```

## Stop and delete
```
docker rm -f db_serv
docker rm -f db_view
```

## Mot de passe pas en clair dans un fichier
```
docker run -d -e POSTGRES_PASSWORD="pwd" --name db_serv --network net oxynep/cours/tp1_db
```

## Init db
dans le Dockerfile
```
COPY sql/01-create.sql /docker-entrypoint-initdb.d
COPY sql/02-init.sql /docker-entrypoint-initdb.d
```

## Persistance
```
docker run -d --name db_serv -v /tmp/data:/var/lib/postgresql/data --network net oxynep/cours/tp1_db
```

# BACK
## Build
```
docker build -t oxynep/cours/tp1_back .
```

## Run
```
docker run --name back_hw oxynep/cours/tp1_back
```

## Stop and delete
```
docker rm -f back_hw
```

## Run Api
```
docker run -d -e POSTGRES_PASSWORD="pwd" --name db_serv --network net oxynep/cours/tp1_db
docker run -d --name back_hw -p 8081:8080 --network net oxynep/cours/tp1_back
```


# HTTP
## Build
```
docker build -t oxynep/cours/tp1_front .
```

## Run
```
docker run -dit --name front -p 8080:80 --network net oxynep/cours/tp1_front
```

## Get la config
```
docker run --rm httpd:2.4 cat /usr/local/apache2/conf/httpd.conf > my-httpd.conf
```
modifier le fichier si besoin

## Set la config
dans le docker file
```
COPY ./my-httpd.conf /usr/local/apache2/conf/httpd.conf
```

# Reverse proxy
## Build
```
docker build -t oxynep/cours/tp1_front .
```

## Run
```
docker run -d -e POSTGRES_PASSWORD="pwd" --name db_serv --network net oxynep/cours/tp1_db
docker run -d --name back_hw --network net oxynep/cours/tp1_back
docker run -d --name front -p 80:80 --network net oxynep/cours/tp1_front
```


# Docker-compose
## Build and Run
```
docker-compose build
docker-compose up
docker-compose up --build
```

## Restart
```
docker-compose restart [nom]
```

## Stop
```
docker-compose down
```


# Question
## Multistage build
Ne pas garder des outils non nécessaire au run.<br>
D'abords on compile avec un JDK puis on run le code avec un JRE

# Publish
```
docker tag oxynep/cours/tp1_db oxynep/cours/tp1_db:1.0
docker tag oxynep/cours/tp1_back oxynep/cours/tp1_back:1.0
docker tag oxynep/cours/tp1_front oxynep/cours/tp1_front:1.0

docker push oxynep/cours/tp1_db:1.0
docker push oxynep/cours/tp1_back:1.0
docker push oxynep/cours/tp1_front:1.0
```
