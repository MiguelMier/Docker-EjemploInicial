# DOCKER CLI

## Lanzar un contenedor de Ubuntu a partir de una imagen ya existente

https://hub.docker.com/

```
docker run ubuntu
docker run -it ubuntu
```

## Lanzar un contenedor de Nginx accediendo al puerto expuesto

```
docker run nginx
docker run -p 80:80 nginx
docker run -d -p 80:80 nginx
docker stop ...
docker ps
docker ps -a
docker run --name web_test -d -p 8080:80 nginx
docker logs web_test
docker logs -f web_test
docker run --name web_test2 -d -p 8081:80 nginx
```

## Lanzar un contenedor de Nginx acceder mediante una Shell y editar el contenido del HTML

```
docker exec -it web_test2 bash
apt update
apt install vim -y
vim /usr/share/nginx/html/index.html
docker stop web_test web_test2
```

# DOCKERFILE

## Construir una imagen de un proyecto en JavaScript y lanzar su contendor para ver el resultado

```
docker build -t app:v1 .
docker run -p 3000:3000 app:v1
```

## Lanzar un contenedor del proyecto web mapeando la base de datos SQLite para tener persistencia

```
docker run -d -p 3000:3000 -v sqlite_data:/etc/todos app:v1
docker run -d -p 3000:3000 -v /mnt/c/sqlite_data:/etc/todos app:v1
docker run -d -p 3000:3000 -v "%cd%"/sqlite_data:/etc/todos app:v1
docker run -d -p 3000:3000 -v "$PWD"/sqlite_data:/etc/todos app:v1
```

## Lanzar un contenedor de MariaDB con parámetros básicos para configurarlo

```
docker run --name mariadb_test -dp 3306:3306 -e MARIADB_ROOT_PASSWORD=password -e MARIADB_DATABASE=todos -d mariadb
docker exec -it maria_test bash
```

## Crear una red para comunicar el contenedor del proyecto web con la base de datos de MariaDB

```
docker network ls
docker network create todo-net
docker run -d --network todo-net --network-alias mariadb -v mariadb-data:/var/lib/mysql -e MARIADB_ROOT_PASSWORD=password -e MARIADB_DATABASE=todos mariadb:11.3.2
docker run -p 3000:3000 --network todo-net -e MYSQL_HOST=mariadb -e MYSQL_USER=root -e MYSQL_PASSWORD=password -e MYSQL_DB=todos app:v1
docker network rm todo-net
```

# DOCKER COMPOSE

## Configurar un fichero “yaml” con el servicio del proyecto web y su base de datos MySQL

```
docker compose up
```

## Configurar el fichero “yaml” con dependencia del proyecto web y su base de datos MySQL

```

depends_on:
 - mysql

docker compose up -d
docker compose logs
docker compose logs mysql
docker compose logs app
docker compose down
```

## Configurar el fichero “yaml” con un volumen para la persistencia de datos MySQL

```
volumes:
 - mysql_data:/var/lib/mysql
volumes:
 mysql_data:

docker compose up -d
docker compose down
docker volume ls
```
