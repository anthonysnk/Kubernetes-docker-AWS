- [Docker](#docker)
- [como funciona](#como-funciona)
  - [Componentes DENTRO del circulo de Docker:](#componentes-dentro-del-circulo-de-docker)
  - [Dentro de la arquitectura de Docker encontramos:](#dentro-de-la-arquitectura-de-docker-encontramos)
- [comandos](#comandos)
- [Tags](#tags)
- [Ciclo de vida](#ciclo-de-vida)
  - [Main process](#main-process)
  - [Sub process](#sub-process)
- [Exponiendo contenedores](#exponiendo-contenedores)
- [Bind mounts](#bind-mounts)
- [Volumenes](#volumenes)
- [Insertar y extraer archivos de un contenedor](#insertar-y-extraer-archivos-de-un-contenedor)
- [imágenes](#imágenes)
- [Construyendo images](#construyendo-images)
- [El sistema de capas](#el-sistema-de-capas)
  - [Instalar dive en ubuntu](#instalar-dive-en-ubuntu)
  - [Usando Docker para desarrollar aplicaciones](#usando-docker-para-desarrollar-aplicaciones)
- [Aprovechando el caché de capas para estructurar correctamente tus imágenes](#aprovechando-el-caché-de-capas-para-estructurar-correctamente-tus-imágenes)
- [Docker networking: colaboración entre contenedores](#docker-networking-colaboración-entre-contenedores)
- [Docker Compose: la herramienta todo en uno](#docker-compose-la-herramienta-todo-en-uno)
  - [comando](#comando)
- [Compose en equipo: override](#compose-en-equipo-override)
- [Administrando tu ambiente de Docker](#administrando-tu-ambiente-de-docker)
- [Deteniendo contenedores correctamente: SHELL vs. EXEC](#deteniendo-contenedores-correctamente-shell-vs-exec)
- [Contenedores ejecutables: ENTRYPOINT vs CMD](#contenedores-ejecutables-entrypoint-vs-cmd)
- [Multi-stage build](#multi-stage-build)
- [Docker-in-Docker](#docker-in-docker)

# Docker

“Docker te permite construir, distribuir y ejecutar cualquier aplicación en cualquier lado.”

Problemáticas del desarrollo de software

1. Construir - Escribir código en la máquina del desarrollador. (Compile, que no compile, arreglar el bug, compartir código, etc. )
Problemática:

Entorno de desarrollo (paquetes)
Dependencias (Frameworks, bibliotecas)
Versiones de entornos de ejecución (runtime, versión Node)
Equivalencia de entornos de desarrollo (compartir el código)
Equivalencia con entornos productivos (pasar a producción)
Servicios externos (integración con otros servicios ejem: base de datos)

2. Distribuir - Llevar la aplicación donde se va a desplegar (Transformarse en un artefacto)

Problemática:

Output de build heterogeo (múltiples compilaciones)
Acceso a servidores productivos (No tenemos acceso al servidor)
Ejecución nativa vs virtualizada
Entornos Serverless

3. Ejecutar - Implementar la solución en el ambiente de producción (Subir a producción)
El reto Hacer que funcione como debería funcionar

Problemática:

Dependencia de aplicación (paquetes, runtime)
Compatibilidad con el entorno productivo (sistema operativo poco amigable con la solución)
Disponibilidad de servicios externos (Acceso a los servicios externos)
Recursos de hardware (Capacidad de ejecución - Menos memoria, procesador más debil)

# como funciona

## Componentes DENTRO del circulo de Docker:

- `Docker daemon`: Es el centro de docker, el corazón que gracias a el, podemos comunicarnos con los servicios de docker.
- `REST API`: Como cualquier otra API, es la que nos permite visualizar docker de forma “gráfica”.
- `Cliente de docker`: Gracias a este componente, podemos comunicarnos con el corazón de docker (Docker Daemon) que por defecto es la línea de comandos.
  
## Dentro de la arquitectura de Docker encontramos:

- `Contenedores`: Es la razón de ser de Docker, es donde podemos encapsular nuestras imagenes para llevarlas a otra computadora, o servidor, etc.
- `Imagenes`: Son las encapsulaciones de x contenedor. Podemos correr nuestra aplicación en Java por medio de una imagen, podemos utilizar Ubuntu para correr nuestro proyecto, etc.
- `Volumenes de datos`: Podemos acceder con seguridad al sistema de archivos de nuestra máquina.
- `Redes`: Son las que permiten la comunicación entre contenedores.

# comandos

```bash 
docker run hello-world (corro el contenedor hello-world)
docker ps (muestra los contenedores activos)
docker ps -a (muestra todos los contenedores)
docker inspect <containe ID> (muestra el detalle completo de un contenedor)
docker inspect <name> (igual que el anterior pero invocado con el nombre)
docker run –-name hello-platzi hello-world (le asigno un nombre custom “hello-platzi”)
docker rename hello-platzi hola-platzy (cambio el nombre de hello-platzi a hola-platzi)
docker rm <ID o nombre> (borro un contenedor)
docker container prune (borro todos lo contenedores que esten parados)
```

# Tags

```bash
docker tag $imagen $imagen:myTag
```

# Ciclo de vida

Cada vez que un contendor se ejecuta, en realidad lo que ejecuta es un proceso del sistema operativo. Este proceso se le conoce como Main process.

## Main process

Determina la vida del contenedor, un contendor corre siempre y cuando su proceso principal este corriendo.

## Sub process

Un contenedor puede tener o lanzar procesos alternos al main process, si estos fallan el contenedor va a seguir encedido a menos que falle el main.

Ejemplos manejados en el video

Batch como Main process
Agujero negro (/dev/null) como Main process

```bash
docker run --name alwaysup -d ubuntu tail -f /dev/null 
```

- el ouput que te regresa es el ID del contentedor _

Te puedes conectar al contenedor y hacer cosas dentro del él con el siguiente comando (sub proceso)

docker exec -it alwaysup bash
Se puede matar un Main process desde afuera del contenedor, esto se logra conociendo el id del proceso principal del contenedor que se tiene en la maquina. Para saberlo se ejecuta los siguientes comandos;

```bash
docker inspect --format '{{.State.Pid}}' alwaysup
```

- El output del comando es el process ID (2474) _

Para matar el proceso principal del contenedor desde afuera se ejecuta el siguiente comando (solo funciona en linux)

```bash
Kill  2474
```

# Exponiendo contenedores

```bash
docker run -d --name proxy nginx (corro un nginx)
docker stop proxy (apaga el contenedor)
docker rm proxy (borro el contenedor)
docker rm -f <contenedor> (lo para y lo borra)
docker run -d --name proxy -p 8080:80 nginx (corro un nginx y expongo el puerto 80 del contenedor en el puerto 8080 de mi máquina)
localhost:8080 (desde mi navegador compruebo que funcione)
docker logs proxy (veo los logs)
docker logs -f proxy (hago un follow del log)
docker logs --tail 10 -f proxy (veo y sigo solo las 10 últimas entradas del log)
```

# Bind mounts

```bash
$ mkdir dockerdata (creo un directorio en mi máquina)
$ docker run -d --name db mongo
$ docker ps (veo los contenedores activos)
$ docker exec -it db bash (entro al bash del contenedor)
$ mongo (me conecto a la BBDD)

shows dbs (listo las BBDD)
use platzi ( creo la BBDD platzi)
db.users.insert({“nombre”:“guido”}) (inserto un nuevo dato)
db.users.find() (veo el dato que cargué)

$ docker run -d --name db -v <path de mi maquina>:<path dentro del contenedor(/data/db mongo)> (corro un contenedor de mongo y creo un bind mount)
```

# Volumenes

```bash
$ docker volume ls (listo los volumes)
$ docker volume create dbdata (creo un volume)
$ docker run -d --name db --mount src=dbdata,dst=/data/db mongo (corro la BBDD y monto el volume)
$ docker inspect db (veo la información detallada del contenedor)
$ mongo (me conecto a la BBDD)
    shows dbs (listo las BBDD)
    use platzi ( creo la BBDD platzi)
    db.users.insert({“nombre”:“guido”}) (inserto un nuevo dato)
    db.users.find() (veo el dato que cargué)
```

# Insertar y extraer archivos de un contenedor

```bash
$ touch prueba.txt (creo un archivo en mi máquina)
$ docker run -d --name copytest ubuntu tail -f /dev/null (corron un ubuntu y le agrego el tail para que quede activo)
$ docker exec -it copytest bash (entro al contenedor)
$ mkdir testing (creo un directorio en el contenedor)
$ docker cp prueba.txt copytest:/testing/test.txt (copio el archivo dentro del contenedor)
$ docker cp copytest:/testing localtesting (copio el directorio de un contenedor a mi máquina)
con “docker cp” no hace falta que el contenedor esté corriendo
```

# imágenes

```bash
$ docker image ls (veo las imágenes que tengo localmente)
$ docker pull ubuntu:20.04 (bajo la imagen de ubuntu con una versión específica
```

# Construyendo images

```bash
$ mkdir imagenes (creo un directorio en mi máquina)
$ cd imagenes (entro al directorio)
$ touch Dockerfile (creo un Dockerfile)
$ code . (abro code en el direcotrio en el que estoy)

##Contenido del Dockerfile##
FROM ubuntu:latest
RUN touch /ust/src/hola-platzi.txt (comando a ejecutar en tiempo de build)
##fin##

$ docker build -t ubuntu:platzi . (creo una imagen con el contexto de build <directorio>)
$ docker run -it ubuntu:platzi (corro el contenedor con la nueva imagen)
$ docker login (me logueo en docker hub)
$ docker tag ubuntu:platzi miusuario/ubuntu:platzy (cambio el tag para poder subirla a mi docker hub)
$ docker push miusuario/ubuntu:platzi (publico la imagen a mi docker hub)
```

# El sistema de capas

## Instalar dive en ubuntu

wget https://github.com/wagoodman/dive/releases/download/v0.9.2/dive_0.9.2_linux_amd64.deb

```bash
sudo apt install ./dive_0.9.2_linux_amd64.deb
$ docker history ubuntu:platzi (veo la info de como se construyó cada capa)
$ dive ubuntu:platzi (veo la info de la imagen con el programa dive)
```

## Usando Docker para desarrollar aplicaciones

```bash
    $ git clone https://github.com/platzi/docker
    $ docker build -t platziapp . (creo la imagen local)
    $ docker image ls (listo las imagenes locales)
    $ docker run --rm -p 3000:3000 platziapp (creo el contenedor y cuando se detenga se borra, lo publica el puerto 3000)
    $ docker ps (veo los contenedores activos)
```

# Aprovechando el caché de capas para estructurar correctamente tus imágenes

```Dockerfile
FROM node:14

COPY [“package.json”, “package-lock.json”, “/usr/src/”]

WORKDIR /usr/src

RUN npm install

COPY [".", “/usr/src/”]

EXPOSE 3000

CMD [“npx”, “nodemon”, “index.js”]
```

```Dockerfile
FROM node:12

COPY [“package.json”, “package-lock.json”, “/usr/src/”]

WORKDIR /usr/src

RUN npm install

COPY [".", “/usr/src/”]

EXPOSE 3000

CMD [“node”, “index.js”]
```

$ docker build platziapp . (creo la imagen local)
$ docker run --rm -p 3000:3000 -v pathlocal/index.js:pathcontenedor/index.js platziapp (corro un contenedor y monto el archivo index.js para que se actualice dinámicamente con nodemon que está declarado en mi Dockerfile)

# Docker networking: colaboración entre contenedores

```bash
$ docker network ls (listo las redes)
$ docker network create --attachable plazinet (creo la red)
$ docker inspect plazinet (veo toda la definición de la red creada)
$ docker run -d --name db mongo (creo el contenedor de la BBDD)
$ docker network connect plazinet db (conecto el contenedor “db” a la red “platzinet”)
$ docker run -d -name app -p 3000:3000 --env MONGO_URL=mondodb://db:27017/test platzi (corro el contenedor “app” y le paso una variable)
$ docker network connect plazinet app (conecto el contenedor “app” a la red “plazinet”)
```

# Docker Compose: la herramienta todo en uno

```Dockerfile
# Establece la imagen base
FROM node

# Crear directorio de trabajo
RUN mkdir -p /opt/app

# Se estable el directorio de trabajo
WORKDIR /opt/app

# Instala los paquetes existentes en el package.json
COPY ./project/package.json .

RUN npm install --quiet

# Copia la Aplicación
COPY ./project/. .

# Instalación de Nodemon en forma Global
# Al realizarse cambios reiniciar el servidor
RUN npm install nodemon -g --quiet

# Expone la aplicación en el puerto 8000
EXPOSE 8000

# Inicia la aplicación al iniciar al contenedor
CMD nodemon -L --watch . app.js
```

```yaml
version: '3.7'
services:

  web:
    container_name: node
    build: .
    depends_on:
      - mongo
    restart: always
    ports:
      - 3000:3000
      - 8000:8000
    volumes:
      - /Users/miruta/Developer/curse-node/node-mongo/project:/opt/app
    networks:
      - redinterna

  mongo:
    container_name: mongo
    image: bitnami/mongodb
    restart: always
    ports:
      - 27017:27017
    volumes:
      - /Users/miruta/Developer/curse-node/node-mongo/mongodata:/bitnami
    networks:
      - redinterna

networks:
  redinterna:
    driver: bridge```
```

## comando

```bash
$ docker network ls (listo las redes)
$ docker network inspect docker_default (veo la definición de la red)
$ docker-compose logs (veo todos los logs)
$ docker-compose logs app (solo veo el log de “app”)
$ docker-compose logs -f app (hago un follow del log de app)
$ docker-compose exec app bash (entro al shell del contenedor app)
$ docker-compose ps (veo los contenedores generados por docker compose)
$ docker-compose down (borro todo lo generado por docker compose)
```

# Compose en equipo: override

```bash
$ touch docker-compose.override.yml (creo el archivo override)
$ docker-compose up -d (crea los servicios/contenedores)
$ docker-compose exec app bash (entro al bash del contenedor app)
$ docker-compose ps (veo los contenedores del compose)
$ docker-compose up -d --scale app=2 (escalo dos instancias de app, previamente tengo que definir un rango de puertos en el archivo compose)
$ docker-compose down (borro todo lo creado con compose)
```

```Dockerfile
######docker-compose.override.yml######

version: "3.8"
services:
  app:
    build: .
    environment:
      UNA_VARIABLE: "Hola platzi"

#######################################
```

```Dockerfile
######docker-compose.yml######

version: "3.8"
services:
  app:
    image: platziapp
    environment:
      MONGO_URL: "mongodb://db:27017/test"
    depends_on:
      - db
    ports:
      - "3000:3000"
  db:
    image: mongo

#######################################
```

# Administrando tu ambiente de Docker

```bash
$ docker ps -a (veo todos los contenedores de mi máquina)
$ docker container prune (borra todos los contenedores inactivos)
$ docker rm -f $(docker ps -aq) (borra todos los contenedores que estén corriendo o apagados)
$ docker network ls (lista todas las redes)
$ docker volume ls (lista todos los volumes)
$ docker image ls (lista todas las imágenes)
$ docker system prune (borra todo lo que no se esté usando)
$ docker run -d --name app --memory 1g platziapp (limito el uso de memoria)
$ docker stats (veo cuantos recursos consume docker en mi sistema)
$ docker inspect app (puedo ver si el proceso muere por falta de recursos)
```

# Deteniendo contenedores correctamente: SHELL vs. EXEC

```bash
$ docker build -t loop . (construyo la imagen)
$ docker run -d --name looper loop (corro el contenedor)
$ docker stop looper (le envía la señal SIGTERM al contenedor)
$ docker ps -l (muestra el ps del último proceso)
$ docker kill looper (le envía la señal SIGKILL al contenedor)
$ docker exec looper ps -ef (veo los procesos del contenedor)
```

# Contenedores ejecutables: ENTRYPOINT vs CMD

https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

```bash
$ docker buils -t ping . (construyo la imagen)
$ docker run --name pinger ping <hostname> (ahora le puedo pasar un parámetro, previamente tengo que agregar el ENTRYPOINT en el Dockerfile)
```

```Dockerfile
FROM ubuntu:trusty
CMD ["/bin/ping", "-c", "3","localhost"]
```

El parametro que enviamos por linea de comandos sobreescribe
`"/bin/ping", "-c", "3","localhost"`

```Dockerfile
FROM ubuntu:trusty
ENTRYPOINT [ "/bin/ping", "-c", "3"]
CMD ["localhost"]
```

De esta manera solo se sobreescribe localhost

# Multi-stage build

```Dockerfile
# Define una "stage" o fase llamada builder accesible para la siguiente fase
FROM node:12 as builder
# copiamos solo los archivos necesarios
COPY ["package.json", "package-lock.json", "/usr/src/"]

WORKDIR /usr/src
# Instalamos solo las dependencias para Pro definidas en package.json
RUN npm install --only=production

COPY [".", "/usr/src/"]
# instalamos dependencias de desarrollo
RUN npm install --only=development

# Pasamos los tests
RUN npm run test
## Esta imagen esta creada solo para pasar los tests.


# Productive image
FROM node:12

COPY ["package.json", "package-lock.json", "/usr/src/"]

WORKDIR /usr/src
# instar las dependencias de PRO
RUN npm install --only=production

# Copiar  el fichero de la imagen anterior.
# De cada stage se reutilizan las capas que son iguales.
COPY --from=builder ["/usr/src/index.js", "/usr/src/"]
# Pone accesible el puerto
EXPOSE 3000

CMD ["node", "index.js"]
### En tiempo de build en caso de que algún paso falle, el build se detendrá por completo.

```

# Docker-in-Docker

```bash
$ docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock docker:19.03.12
$ docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock -v $(wich docker):/bin/docker wagoodman/dive:latest prodapp
```