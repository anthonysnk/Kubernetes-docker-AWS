- [NOTAS](#notas)
- [Debugear docker](#debugear-docker)
- [Instalacion de Docker](#instalacion-de-docker)
  - [Crear un grupo y usuario para el docker](#crear-un-grupo-y-usuario-para-el-docker)
- [Instalacion de Kubernet](#instalacion-de-kubernet)
- [Instalacion de miniKube](#instalacion-de-minikube)
- [Que no es docker](#que-no-es-docker)
- [Comandos](#comandos)
  - [Creando una imagen](#creando-una-imagen)
  - [Exponer un puerto desde el Dockerfile](#exponer-un-puerto-desde-el-dockerfile)
  - [Construyendo la IMAGEN](#construyendo-la-imagen)
  - [Borrar una imagen](#borrar-una-imagen)
  - [Obtener la ip de un contenedor desde inspect](#obtener-la-ip-de-un-contenedor-desde-inspect)
  - [crear un contenedor publicando un puerto especifico](#crear-un-contenedor-publicando-un-puerto-especifico)
  - [crear un contenedor publicando un puerto especifico en un interfaz concreto](#crear-un-contenedor-publicando-un-puerto-especifico-en-un-interfaz-concreto)
  - [crear la nueva imagen](#crear-la-nueva-imagen)
  - [taguear como latest](#taguear-como-latest)
  - [crear un contenedor publicando a un puerto local expuesto en la imagen](#crear-un-contenedor-publicando-a-un-puerto-local-expuesto-en-la-imagen)
  - [Crear contenedor y arrancarlo a la vez en segundo plano con un solo comando](#crear-contenedor-y-arrancarlo-a-la-vez-en-segundo-plano-con-un-solo-comando)
  - [Comprobar rango de puertos locales](#comprobar-rango-de-puertos-locales)
  - [Instalar terminator](#instalar-terminator)
  - [Crear nueva division horizontal en terminator](#crear-nueva-division-horizontal-en-terminator)
  - [crear contenedor](#crear-contenedor)
  - [Algunos ejemplos de ejecutar comandos dentro del contenedor con exec](#algunos-ejemplos-de-ejecutar-comandos-dentro-del-contenedor-con-exec)
  - [Ejecutar la shell bash dentro de un contenedor que esta corriendo](#ejecutar-la-shell-bash-dentro-de-un-contenedor-que-esta-corriendo)
  - [Ver logs de nginx dentro del contenedor](#ver-logs-de-nginx-dentro-del-contenedor)



# NOTAS
Comandos para comprobar las extensiones de virtualizacion:

```cat /proc/cpuinfo| egrep "vmx|svm"```

Enlaces de instalacion:

```
https://docs.docker.com/install/linux/docker-ce/ubuntu/

https://kubernetes.io/docs/tasks/tools/install-kubectl/

https://kubernetes.io/docs/tasks/tools/install-minikube/

https://docs.docker.com/compose/install/

https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html
```
# Debugear docker
1. Find out if the Docker daemon is running

Since I am running on Ubuntu, I built this VM to use Upstart as the boot time start mechanism. This means I need to use it’s utilities to check the status:

> ```sudo status docker```

docker start/running, process 2841

so far so good.

2. Start your container, see if it’s running.

> ```sudo docker start xxx ```(where ‘xxx’ are there first 3 characters of the docker container)

> ```sudo docker ps```

When I did this, nothing was listed..

CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES

jenkinsadmin@ubuntu:~/jenkins-data$

so that tells me the container didnt start correctly. Then I did a

> ```sudo docker ps -a```

now i see my container:

CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES

6a721ae2d232 myjenkinsdata “echo ‘Data container” 4 weeks ago Exited (0) About an hour ago jenkins-data

under the STATUS column, I see it is “Exited”. Not good.

3. Check the container log

> ```sudo docker logs xxx```

where ‘xxx’ are the first three characters of the container. This should show you any message output during container initialization. In my case, it was

jenkinsadmin@ubuntu:~$ sudo docker logs 6a7

[sudo] password for jenkinsadmin:
Data container for Jenkins

this “data container for jenkins” message corresponds to the final entry in my Dockerfile,

CMD [“echo”, “Data container for Jenkins”]

that at least told me the commands in the Dockerfile were executing.

4. Commit the container changes to a new image

The idea here is that if there is corruption in the container, you can determine whether the image itself is also at fault by committing the changes (to a new image) and creating a new container from that new image.

>docker commit –change “debug container vs image” xxxxxxxxx <repo>/<image>:<version> where ‘xxxxxxxxxxx’ is the container id.

now you can execute the “docker run” command (as you did originally to create the container you are trying to debug)

# Instalacion de Docker
El enlace de la docuementacion de docker es
https://docs.docker.com/engine/install/ubuntu/

Borrar cualquier version que tuviecemos instalada 
```sudo apt-get remove docker docker-engine docker.io containerd runc```

## Crear un grupo y usuario para el docker
Este comando debe correrce en el root,  y debemos asiganar en user el usuario que deseamos agregar al grupo llamado docker
```usermod -a -G docker user```

# Instalacion de Kubernet
https://kubernetes.io/docs/tasks/tools/install-kubectl/

Dar permiso de ejecucion al archivo descargado
```chmod u+x kubectl```
Moveremos el archivo a los binarios
```sudo mv kubectl /usr/local/bin```

# Instalacion de miniKube
https://kubernetes.io/docs/tasks/tools/install-minikube/

Igualmente lo movemos a user local bin
```sudo mv minikube /usr/local/bin```

# Que no es docker
 - NO es Gestor de entorno virtuales de desarrollo (vagrant)
 - NO es Un software de vistulizacion 
 - NO es Un gestor de configuracion
 - NO es  Un software de contenedores

Es un ecosistrma basado en un software del mismo nombre, donde podemos gestionar contenedores y sus imaganes, ademas de compartirlas

Software para montar entornos de desarrollo, un aherramienta para jcutar aplicaciones aislandolas de otras, algo para ejcutar exactamente los mismo en varios entornos, etc. Permite evitar el overhead.

Se maneja por capaz lo que agiliza y optimiza los recursos.
por ejemplo en la primera capa instalamos un apache, nuestra imagen 1.0 y luego instalamos un git, ```git``` caeria en la sobre la capa de apache resultando una imagen 1.1, sin reescribir la capa 1.

# Comandos
https://docs.docker.com/engine/reference/commandline/docker/

- ```docker images```:    Lista las imagenes que tenemos almacenadas en la cache. 
- ```docker build RUTA```:  Construye la imagen  apartir de nuestro archivo Dockerfile
- ```docker image rm ID-IMAGEN```: Borrar una imagen docker
- ```docker build -t tagName:tagValue .``` : Para ponerle etiquetas a nuestas imagenes, sirve para versionar las imaganes
- ```docker tag helloworld:1.0 helloworld:lastest``` : Reetiquetar para indicarle que esta version es la ultima. Etiquetar la imagen como latest
- ```docker inspect idIMAGE```: Conocer los metadatos de las imagenes
- ```docker inspect helloworld:lastest```: Otra manera de ver los metadatos
- ```docker create helloworld``` o ```docker create helloworld:etiqueta``` : crear un contenedor
- ```docker ps```: listar contenedores en ejecucion
- ```docker ps -a```: listar contenedores (incluso parados)
- ```docker rm ID_parcial```, ```docker rm ID```, ```docker rm name```: eliminar contenedores
- ```docker start name``` : Iniciar o arrancar un contenedor ya creado
- ```sudo docker exec -it ID bash```: run de docker
- ```docker stop name ```: parar un contenedor ya creado y arrancado
- ```docker rm name -f ```: eliminar un contenedor que esta corriendo
- ```docker create --name web01 helloworld:latest``` : crear un container con nombre
- ```docker inspect web01 ```: Inspeccionar contenedor
- ```sudo docker logs xxx```: Para ver los errores de un docker

## Creando una imagen
```Dockerfile```

```
FROM ubuntu:20.04 #obteniendo una imagen base

RUN apt-get update
RUN apt-get install nginx -y
RUN echo 'hello world' > /var/www/html/index.html

CMD ["nginx", "-g", "daemon off;"] 
# comando principal por defecto para correc contenedores
```

## Exponer un puerto desde el Dockerfile
```
FROM ubuntu:20.04 #obteniendo una imagen base

RUN apt-get update
RUN apt-get install nginx -y
RUN echo 'hello world' > /var/www/html/index.html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"] 
```

## Construyendo la IMAGEN 

```docker build RUTA```

## Borrar una imagen 

```docker image rm ID-IMAGEN```
## Obtener la ip de un contenedor desde inspect
```
docker inspect NAME|grep IPAddress
```

## crear un contenedor publicando un puerto especifico

```
docker create -p 8080:80 --name web01 helloworld:latest
```

## crear un contenedor publicando un puerto especifico en un interfaz concreto

```
docker create -p ip:8080:80 --name web01 helloworld:latest
```

## crear la nueva imagen 

```
docker build -t helloworld:2.0
```

## taguear como latest

```
docker tag helloworld:2.0 helloworld:latest
```

## crear un contenedor publicando a un puerto local expuesto en la imagen

```
docker create -P --name web01 helloworld:latest
```

## Crear contenedor y arrancarlo a la vez en segundo plano con un solo comando

```
docker run -d ...
```

## Comprobar rango de puertos locales

```
cat /proc/sys/net/ipv4/ip_local_port_range
```
## Instalar terminator

```
sudo apt install terminator -y
```

## Crear nueva division horizontal en terminator
```
Ctrl + Shift + o
```

## crear contenedor
```
docker run --name web01 -d -P helloworld
```

## Algunos ejemplos de ejecutar comandos dentro del contenedor con exec
```
docker exec web01 ls /
docker exec web01 ps
```

## Ejecutar la shell bash dentro de un contenedor que esta corriendo
```
docker exec -it web01 bash
```

## Ver logs de nginx dentro del contenedor
```
tail -f /var/log/nginx/access.log
```