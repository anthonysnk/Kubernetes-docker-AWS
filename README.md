<p align="center">
<h2>Kubernetes y Docker en AWS</h2>
  <img src="https://www.ayies.com/wp-content/uploads/2018/07/docker-kubernetes-logo.png">
</p>

- [NOTAS](#notas)
- [Debugear docker](#debugear-docker)
- [Instalacion de Docker](#instalacion-de-docker)
  - [Crear un grupo y usuario para el docker](#crear-un-grupo-y-usuario-para-el-docker)
- [Instalacion de Kubernet](#instalacion-de-kubernet)
- [Instalacion de miniKube](#instalacion-de-minikube)
- [Que no es docker](#que-no-es-docker)
- [Comandos Docker](#comandos-docker)
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
  - [listar volumenes](#listar-volumenes)
  - [Crear volumen](#crear-volumen)
  - [Inspeccionar volumen](#inspeccionar-volumen)
  - [crear contenedor con volumen persistente](#crear-contenedor-con-volumen-persistente)
  - [eliminar un volumen](#eliminar-un-volumen)
  - [limpiar o purgar el listado de volumenes no utilizados](#limpiar-o-purgar-el-listado-de-volumenes-no-utilizados)
  - [listar redes](#listar-redes)
  - [Crear network](#crear-network)
  - [Crear un contenedor dentro de una red](#crear-un-contenedor-dentro-de-una-red)
  - [filtrar ip en la salida de docker inspect](#filtrar-ip-en-la-salida-de-docker-inspect)
  - [eliminar las redes que no esten en uso](#eliminar-las-redes-que-no-esten-en-uso)
  - [conectar y desconectar un contenedor de una red](#conectar-y-desconectar-un-contenedor-de-una-red)
  - [docker login](#docker-login)
  - [etiquetar imagen](#etiquetar-imagen)
  - [subir imagen](#subir-imagen)
  - [descargar imagen](#descargar-imagen)
  - [eliminar todas las imagenes](#eliminar-todas-las-imagenes)
  - [eliminat todos los contenedores](#eliminat-todos-los-contenedores)
  - [Leer variables de entorno](#leer-variables-de-entorno)
  - [leer contenido de la variable de entorno SHELL](#leer-contenido-de-la-variable-de-entorno-shell)
  - [crear contenedor inyectando variables de entorno](#crear-contenedor-inyectando-variables-de-entorno)
  - [siempre tagear las imagaenes](#siempre-tagear-las-imagaenes)
- [Docker compose](#docker-compose)
  - [Levantar contenedor o contenedores definidos con docker compose en primer plano](#levantar-contenedor-o-contenedores-definidos-con-docker-compose-en-primer-plano)
  - [Levantar contenedor o contenedores definidos con docker compose en segundo plano](#levantar-contenedor-o-contenedores-definidos-con-docker-compose-en-segundo-plano)
  - [Parar contenedores definidos con docker compose](#parar-contenedores-definidos-con-docker-compose)
  - [Eliminar contendores creados con docker compose](#eliminar-contendores-creados-con-docker-compose)
  - [ver los logs](#ver-los-logs)
  - [Construir imagen a traves de docker-compose](#construir-imagen-a-traves-de-docker-compose)
- [Pasarle archivos al docker](#pasarle-archivos-al-docker)
  - [Parte 1](#parte-1)
  - [Crear network](#crear-network-1)
  - [Crear contenedor de pruebas con mariadb:latest1](#crear-contenedor-de-pruebas-con-mariadblatest1)
  - [Conectarnos al contenedor y comprobar la variable de entorno](#conectarnos-al-contenedor-y-comprobar-la-variable-de-entorno)
    - [Conectarnos a mysql y listar bases de datos](#conectarnos-a-mysql-y-listar-bases-de-datos)
    - [conectar a la base de datos desde otro contenedor y listar bases de datos](#conectar-a-la-base-de-datos-desde-otro-contenedor-y-listar-bases-de-datos)
    - [Importamos la base de datos](#importamos-la-base-de-datos)
    - [conectar a la base de datos desde otro contenedor y listar los cambios](#conectar-a-la-base-de-datos-desde-otro-contenedor-y-listar-los-cambios)
  - [Parte 2](#parte-2)
  - [Crear la imagen de nuestra aplicacion](#crear-la-imagen-de-nuestra-aplicacion)
  - [Ejecutar nuestra imagen](#ejecutar-nuestra-imagen)
  - [Conectamos a la base de datos e insertamos nuevos datos](#conectamos-a-la-base-de-datos-e-insertamos-nuevos-datos)
- [miniKube install with virtualbox](#minikube-install-with-virtualbox)
  - [Comandos minikube](#comandos-minikube)
  - [configuraciones](#configuraciones)
  - [Troubleshootinde del cluster con Minikube](#troubleshootinde-del-cluster-con-minikube)
  - [Plugins de minikube (addons)](#plugins-de-minikube-addons)
- [Kubectl](#kubectl)
  - [comandos](#comandos)
  - [Manifests VS Linea de comandos](#manifests-vs-linea-de-comandos)
    - [Implementacion de manifests](#implementacion-de-manifests)
    - [Borrar Manifests](#borrar-manifests)
    - [Comandos Con manifest:](#comandos-con-manifest)
    - [Comandos Sin manifest:](#comandos-sin-manifest)
  - [Pods](#pods)
    - [Ejemplo de post con manisfest](#ejemplo-de-post-con-manisfest)
  - [Metadata, Labels y Selectors](#metadata-labels-y-selectors)



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

# Comandos Docker
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

```BASH
FROM ubuntu:20.04 #obteniendo una imagen base

RUN apt-get update
RUN apt-get install nginx -y
RUN echo 'hello world' > /var/www/html/index.html

CMD ["nginx", "-g", "daemon off;"] 
# comando principal por defecto para correc contenedores
```
## Exponer un puerto desde el Dockerfile
```BASH
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
## listar volumenes

```
docker volume ls
```
## Crear volumen
```
docker volume create my-volume
```
## Inspeccionar volumen
```
docker volume inspect my-volume
```
## crear contenedor con volumen persistente
```
docker create -P --mount source=my-volume,target=/var/www/html --name web01 helloworld:latest
```
## eliminar un volumen
```
docker volume rm my-volume
```
## limpiar o purgar el listado de volumenes no utilizados
``` 
docker volume prune
```
## listar redes

```
docker network ls
```
## Crear network

```
docker network create nombre_de_la_red --driver nombre_del_driver 
por ejemplo:
docker network create web --driver bridge
```

## Crear un contenedor dentro de una red

```
docker create --name web01 --network web helloworld
```
##  filtrar ip en la salida de docker inspect
```
docker inspect web01 |grep IPAddress
```
## eliminar las redes que no esten en uso

```
docker network prune
```
## conectar y desconectar un contenedor de una red

```
docker connect web web01
docker disconnect web web01
```
## docker login
```docker login```
## etiquetar imagen

```
docker tag imagenlocal:etiqueta usuarioremoto/imagenremota:etiqueta
```
## subir imagen

```
docker push usuarioremoto/imagenremota:etiqueta
```
## descargar imagen

```
docker pull usuarioremoto/imagen:etiqueta
```
## eliminar todas las imagenes
```docker rmi -f $(docker images -q)```
## eliminat todos los contenedores
```docker rm $(docker ps -a -q)```
## Leer variables de entorno 

``` env ```
## leer contenido de la variable de entorno SHELL

``` echo $SHELL ```

## crear contenedor inyectando variables de entorno

``` 
docker create -e variable1=valor1 -e variable2=valor2 imagen:tag
``` 
## siempre tagear las imagaenes

```
docker tag helloworld:4.0 helloworld:latest
docker tag helloworld:4.0 anthonysnk/helloworld:4.0
docker tag helloworld:4.0 anthonysnk/helloworld:latest
```

# Docker compose
Con esta herramientas podemos ejecutar entornos multi contenedor.
https://docs.docker.com/compose/
## Levantar contenedor o contenedores definidos con docker compose en primer plano

```
docker-compose up
```
## Levantar contenedor o contenedores definidos con docker compose en segundo plano

```
docker-compose up -d
```
## Parar contenedores definidos con docker compose
```
docker-compose down
```
## Eliminar contendores creados con docker compose
```
docker-compose rm
```
## ver los logs
```
docker-compose logs -f
```
## Construir imagen a traves de docker-compose
```
docker-compose build
```

# Pasarle archivos al docker
En caso de el nginx  es con el parametro -v
```
docker run -d -p 80:80 --name website -v $(psw):/usr/share/nginx/html
# Laboratorio
```
## Parte 1

```docker pull mariadb```
## Crear network

``` 
docker network create lab
```
## Crear contenedor de pruebas con mariadb:latest1

```
docker run --net lab --name db01 -e MYSQL_ROOT_PASSWORD=password -d mariadb:latest
```
## Conectarnos al contenedor y comprobar la variable de entorno
```
docker exec -it db01 bash
env
```
### Conectarnos a mysql y listar bases de datos
```
mysql -uroot -ppassw0rd
show databases;
```
### conectar a la base de datos desde otro contenedor y listar bases de datos

```
docker run -it --name test --network lab --rm mariadb:latest mysql -hdb01 -uroot -p
show databases;
```
### Importamos la base de datos

```
docker exec -i db01 sh -c 'exec mysql -uroot -p"$MYSQL_ROOT_PASSWORD"' < dump/users.sql
```
### conectar a la base de datos desde otro contenedor y listar los cambios

```
docker run -it --name test --network lab --rm mariadb:latest mysql -hdb01 -uroot -p
show databases;
use users;
show tables;
select * from users;
```
## Parte 2
## Crear la imagen de nuestra aplicacion

``` 
cd app
docker build -t python . 
```
## Ejecutar nuestra imagen

```
docker run --network lab -e db_host=db01 -e db_user=root -e MYSQL_ROOT_PASSWORD=password -e db_name=users --name python --rm -p 5000:5000 python:latest
```
## Conectamos a la base de datos e insertamos nuevos datos

```
docker exec -i db01 bash
mysql -uroot -ppassw0rd
use users;
select * from user;
insert into user values(2,"Warren")
select * from user;
```
# miniKube install with virtualbox
El siguiente comando descargara una iso la cual contendra ducker, y ese docker desplegara ciertos componentes para el cluster funcione

```minikube start --cpu=2 --memory=2GB --driver=virtualbox```

si deseamos eliminar cualquier tipo de chache que pudo quedar

```rm -rf .minikube/```

si no deseamos utilizar virtual box podemos cambiar el driver a docker y este bajara una imagen

```minikube start --cpu=2 --memory=2GB --driver=docker```

## Comandos minikube

`kubectl get nodes`: Devuelve los nodos del cluster

`minikube status`: Devuelve el stado del host, kubelet, apiserver, kubeconfig

`minikube pause`: Este comando pausara el cluster (Similar a hivernar la maquina)

`minikube unpause`: restaurar el cluster a su forma no detenida

`minikube stop`: Detiene por completo el cluster y sus componentes (similiar como apagar la maquina virtual)

`minikube start`: Inicia el cluster y sus componentes

`minikube delete`: Borrara los cluster

`minikube config view`:  muestra la configacion del archivo ./minikube/config/config.json

`minikube config get cpus`: Devolvera los cpu del archivo de configuraciones, puede utilizarse para cualquier parametro

`minikube config unset cpus`: Eliminara los opcion que le pasemos del archivo de configuracion
## configuraciones
Las configuracione se encuentra en la ruta `./minikube/config` el archivo dentro es `config.json`
La opciones que podesmos agregar en este json las podesmos en listar con el comando `minikube config`

Para poder agregar una configuracion 
`minikube set OPCION`
Los efectos no cobraran efecto cuando se elimine el cluster y luego se inicie nuevamente.
```json
{
  "cpus":2,
  "dahboard": false,
  "disk-size": "2000",
  "driver": "virtualbox",
  "insecure-registry": true,
  "memory": 2000
}
```
## Troubleshootinde del cluster con Minikube

`minikube ip`:obteniendo la ip del cluster

`minikube logs`: Logs del nuestro sistema de los diferentes contedores

`minikube logs -f --problems=true`: mostrara los los errores

`minikube update-check`: muestra la version actual y cual es la version nueva

`minikube ssh`: para conectarnos a la maquina virtual

`ps aux`: para observar que esta corriendo dentro de la maquina

`docker ps`: para ver que contenedores tiene ma laquina virtual

`tail -f /var/log/`: para ver los logs

`docker exec -it ID bash`: para ver que esta corriendo dentro de los contenedores

## Plugins de minikube (addons)
No permitiran instalar ciertos componentes.

`minikube addons list`: nos muestran los plugins

`minikube addons enable NombrePlugin`: Para poder activar un plugin

`minikube addons enable dashboard`: para activar el addon de dashboard

`minikube dashboard`: para lanzar el addon

# Kubectl
Es una herramienta de linea de comando que va a permitir controla uno o mas cluster.
La sintaxis de los comando es 
`kubectl <commad> <recurso> <nombre> <argumento>`

Los comando suele ser un verbo que no indica una accion como lo son: `create`, `expose`, `run`, `set`, etc

## comandos

`kubectl get nodes`: Entrega los nodos de nuestro cluster

`kubectl describe node minikube`: para obtener solo el nodo que le indiquemos

`kubectl get node -o wide`: Nos devuelve una salida mas completa de la salida de los nodos

`kubectl get node -o json`: Nos muestra una salida en formato json de la informacion de los recursos

`kubectl describe nodes`: Muestra toda la informacion de los nodos

`kubectl get all`:Devuelve todos los recursos

`kubectl apply -f ejemplo.yaml`: creando manifests

`kubectl delete -f ejemplo.yaml`: Elimina el manisfests indicado

`kubectl apply -f pod1.yaml` : Aplicando el manifest del pod

`kubectl get pods` o `kubectl get pod`: Devuelve los pods

`kubectl get pods -o wide`: devuele mas informacion de pods

`kubectl describe pods`: describir la informacion del pod

`kubectl describe pod nginx`: descriibe la informacion del pod deseado

`kubectl delete pod nginx`: borra un el pod especificado

`kubectl delete -f pod1.yaml`: borrar el pod desde el manisfest
## Manifests VS Linea de comandos
Los manisfest es la donde se almacena la confirguracion del cluster para poder ser creado este puede ser de tipo JSON, o YAML.

```yaml
apiVersion: apps/v1 
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 1
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```
Para implementarlo se corrre el siguiente comando
### Implementacion de manifests

`kubectl apply -f ejemplo.yaml`

### Borrar Manifests

`kubectl delete -f ejemplo.yaml`

### Comandos Con manifest:

```
kubectl apply -f ejemplo.yaml
kubectl get all
kubectl delete -f ejemplo.yaml
```
### Comandos Sin manifest:

```
kubectl run nginx --image=nginx:latest --port 80 --replicas=1
kubectl get all
kubectl delete deployment nginx
```

## Pods

Es el objeto minimo para desplegar en kubernetes
El pods siempre incluira un contenedor
Un pods se inicia y es mas que un contenedor donde correra un cmd, puede correrse hasta que se le ordene parar.

### Ejemplo de post con manisfest

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: webserver
    image: nginx:latest
    ports:
    - containerPort: 80
```

`kubectl delete -f pod1.yaml`: borrar el pod desde el manisfest

## Metadata, Labels y Selectors