<p align="center">
<h2>Kubernetes y Docker en AWS</h2>
</p>
<p align="center">
  <img src="./img/docker-kubernetes-logo.png">
</p>

- [NOTAS](#notas)
- [Guía de redes de Kubernetes para principiantes](#guía-de-redes-de-kubernetes-para-principiantes)
  - [Comunicación entre contenedores en el mismo pod](#comunicación-entre-contenedores-en-el-mismo-pod)
  - [¿Qué es un espacio de nombres de red?](#qué-es-un-espacio-de-nombres-de-red)
  - [Comunicación entre pods en el mismo nodo](#comunicación-entre-pods-en-el-mismo-nodo)
  - [¿Qué es un puente de red?](#qué-es-un-puente-de-red)
  - [Comunicación entre pods en diferentes nodos](#comunicación-entre-pods-en-diferentes-nodos)
  - [Comunicación entre pods y servicios](#comunicación-entre-pods-y-servicios)
  - [¿Cómo funciona el DNS? ¿Cómo descubrimos las direcciones IP?](#cómo-funciona-el-dns-cómo-descubrimos-las-direcciones-ip)
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
    - [Comandos Con manifest](#comandos-con-manifest)
    - [Comandos Sin manifest](#comandos-sin-manifest)
  - [Pods](#pods)
    - [Ejemplo de post con manisfest](#ejemplo-de-post-con-manisfest)
  - [Metadata, Labels y Selectors](#metadata-labels-y-selectors)
  - [comandos selectors](#comandos-selectors)
- [Replication controlers](#replication-controlers)
- [replicaset](#replicaset)
- [Servicios](#servicios)
- [Persisten volume](#persisten-volume)
- [ConfigMaps](#configmaps)
  - [Creando un configMap desde la linea de comandos](#creando-un-configmap-desde-la-linea-de-comandos)
- [Secrets](#secrets)
  - [comandos](#comandos-1)
  - [tipos](#tipos)
- [Deployments](#deployments)
  - [Comandos](#comandos-2)
    - [Available Commands for rolout](#available-commands-for-rolout)
    - [desplegando una imagen](#desplegando-una-imagen)
    - [Volver a una version anterior](#volver-a-una-version-anterior)
- [Namespaces](#namespaces)
  - [Comandos](#comandos-3)
  - [Creacion de un namespace](#creacion-de-un-namespace)
  - [Crear un pot dentro del namespace](#crear-un-pot-dentro-del-namespace)
- [Kubernetes en AWS con Kops](#kubernetes-en-aws-con-kops)
  - [Instalacion en Linux](#instalacion-en-linux)
  - [Comandos](#comandos-4)

# NOTAS

Comandos para comprobar las extensiones de virtualizacion:

```cat /proc/cpuinfo| egrep "vmx|svm"```

Enlaces de instalacion:

```url
https://docs.docker.com/install/linux/docker-ce/ubuntu/

https://kubernetes.io/docs/tasks/tools/install-kubectl/

https://kubernetes.io/docs/tasks/tools/install-minikube/

https://docs.docker.com/compose/install/

https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html

```

# Guía de redes de Kubernetes para principiantes

Hay cinco cosas esenciales que debe comprender sobre las redes en Kubernetes

Comunicación entre contenedores en el mismo pod
Comunicación entre pods en el mismo nodo
Comunicación entre pods en diferentes nodos
Comunicación entre pods y servicios
¿Cómo funciona el DNS? ¿Cómo descubrimos las direcciones IP?

<p align="center">
  <img src="./img/networking-overview.png">
</p>

## Comunicación entre contenedores en el mismo pod

Primero, si tiene dos contenedores ejecutándose en el mismo pod, ¿cómo se comunican entre sí?

Esto sucede a través de localhosty números de puerto. Al igual que cuando ejecuta varios servidores en su propia computadora portátil.

Esto es posible porque los contenedores en el mismo pod están en el mismo espacio de nombres de red: comparten recursos de red.

## ¿Qué es un espacio de nombres de red?
Es una colección de interfaces de red (conexiones entre dos equipos en una red) y tablas de enrutamiento (instrucciones sobre dónde enviar paquetes de red).

Los espacios de nombres son útiles porque puede tener muchos espacios de nombres de red en la misma máquina virtual sin colisiones ni interferencias.

(No querrá que todos sus pods ejecuten contenedores que escuchen en el puerto 3000 en el mismo espacio de nombres; ¡todos colisionarían!)

Hay un contenedor secreto que se ejecuta en cada pod de Kubernetes. El trabajo n. ° 1 de este contenedor es mantener abierto el espacio de nombres en caso de que todos los demás contenedores del pod mueran. Se llama `pause` contenedor.

Entonces, cada pod tiene su propio espacio de nombres de red. Los contenedores del mismo pod están en el mismo espacio de nombres de red. Es por eso que puede hablar entre contenedores a través de localhost y por qué debe estar atento a los conflictos de puertos cuando tiene varios contenedores en el mismo pod.

<p align="center">
  <img src="./img/same-pod.gif">
</p>

## Comunicación entre pods en el mismo nodo

Cada pod de un nodo tiene su propio espacio de nombres de red. Cada pod tiene su propia dirección IP.

Y cada pod piensa que tiene un dispositivo Ethernet totalmente normal llamado `eth0` para realizar solicitudes de red. Pero Kubernetes lo está fingiendo: es solo una conexión Ethernet virtual.

El `eth0` de cada pod está realmente conectado a un dispositivo ethernet virtual en el nodo.

Un dispositivo ethernet virtual es un túnel que conecta la red del pod con el nodo. Esta conexión tiene dos lados: en el lado de la vaina, se llama `eth0`, y en el lado del nodo, se llama `vethX`.

¿Por qué el X? Hay una `vethX` conexión para cada pod del nodo. (Así que estarían `veth1`, `veth2`, `veth3`, `etc`.)

Cuando un pod realiza una solicitud a la dirección IP de otro nodo, realiza esa solicitud a través de su propia `eth0` interfaz. Esto hace un túnel a la `vethX` interfaz virtual respectiva del nodo .

Pero entonces, ¿cómo llega la solicitud al otro grupo?

El nodo usa un puente de red.

## ¿Qué es un puente de red?

Un puente de red conecta dos redes juntas. Cuando una solicitud llega al puente, el puente pregunta a todos los dispositivos conectados (es decir, pods) si tienen la dirección IP correcta para manejar la solicitud original.

(Recuerde que cada pod tiene su propia dirección IP y conoce su propia dirección IP).

Si uno de los dispositivos lo hace, el puente almacenará esta información y también reenviará los datos al respaldo original para que se complete su solicitud de red.

En Kubernetes, este puente se llama `cbr0`. Cada pod de un nodo es parte del puente y el puente conecta todos los pods del mismo nodo.

<p align="center">
  <img src="./img/pods-on-node.gif">
</p>

## Comunicación entre pods en diferentes nodos

Pero, ¿qué pasa si las vainas están en diferentes nodos?

Bueno, cuando el puente de red pregunta a todos los dispositivos conectados (es decir, pods) si tienen la dirección IP correcta, ninguno de ellos dirá que sí.

(Tenga en cuenta que esta parte puede variar según el proveedor de la nube y los complementos de red).

Después de eso, el puente vuelve a la puerta de enlace predeterminada. Esto sube al nivel de clúster y busca la dirección IP.

A nivel de clúster, hay una tabla que asigna rangos de direcciones IP a varios nodos. A los pods en esos nodos se les habrán asignado direcciones IP de esos rangos.

Por ejemplo, podría dar Kubernetes vainas en el nodo 1 como direcciones `100.96.1.1`, `100.96.1.2` etc. Y Kubernetes da vainas en el nodo `2` direcciones como `100.96.2.1`, `100.96.2.2` y así sucesivamente.

Entonces, esta tabla almacenará el hecho de que las direcciones IP que parecen `100.96.1.xxx` deben ir al nodo 1, y las direcciones como `100.96.2.xxx` deben ir al nodo 2.

Una vez que hayamos descubierto a qué nodo enviar la solicitud, el proceso procede aproximadamente de la misma manera que si los pods hubieran estado en el mismo nodo todo el tiempo.

<p align="center">
  <img src="./img/node-to-node.gif">
</p>

## Comunicación entre pods y servicios

Un último patrón de comunicación es importante en Kubernetes.

En Kubernetes, un servicio le permite asignar una única dirección IP a un conjunto de pods. Realiza solicitudes a un punto final (nombre de dominio / dirección IP) y el servicio envía solicitudes a un pod en ese servicio.

Esto sucede a través de `kube-proxy` un pequeño proceso que Kubernetes ejecuta dentro de cada nodo.

Este proceso asigna direcciones IP virtuales a un grupo de direcciones IP de pod reales.

Una vez que se `kube-proxy` ha asignado la IP virtual del servicio a una IP de pod real, la solicitud procede como en las secciones anteriores.

## ¿Cómo funciona el DNS? ¿Cómo descubrimos las direcciones IP?

DNS es el sistema para convertir nombres de dominio a direcciones IP.

Los clústeres de Kubernetes tienen un servicio responsable de la resolución de DNS.

A cada servicio de un clúster se le asigna un nombre de dominio como `my-service.my-namespace.svc.cluster.local.`

Los pods reciben automáticamente un nombre `DNS` y también pueden especificar el suyo propio mediante las propiedades `hostname` y `subdomain` en su configuración YAML.

Entonces, cuando se realiza una solicitud a un servicio a través de su nombre de dominio, el servicio DNS lo resuelve en la dirección IP del servicio.

Luego, `kube-proxy` convierte la dirección IP de ese servicio en una dirección IP de pod. Después de eso, en función de si los pods están en el mismo nodo o en nodos diferentes, la solicitud sigue una de las rutas explicadas anteriormente.
# Debugear docker

1. Find out if the Docker daemon is running

Since I am running on Ubuntu, I built this VM to use Upstart as the boot time start mechanism. This means I need to use it’s utilities to check the status:

> ```sudo status docker```

docker start/running, process 2841

so far so good;

2. Start your container, see if it’s running.

> ```sudo docker start xxx``` (where ‘xxx’ are there first 3 characters of the docker container)

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

>docker commit –change “debug container vs image” xxxxxxxxx `<repo>/<image>:<version>` where ‘xxxxxxxxxxx’ is the container id.

now you can execute the “docker run” command (as you did originally to create the container you are trying to debug)

# Instalacion de Docker

El enlace de la docuementacion de docker es
<https://docs.docker.com/engine/install/ubuntu/>

Borrar cualquier version que tuviecemos instalada
```sudo apt-get remove docker docker-engine docker.io containerd runc```

## Crear un grupo y usuario para el docker

Este comando debe correrce en el root,  y debemos asiganar en user el usuario que deseamos agregar al grupo llamado docker
```usermod -a -G docker user```

# Instalacion de Kubernet

<https://kubernetes.io/docs/tasks/tools/install-kubectl/>

Dar permiso de ejecucion al archivo descargado
```chmod u+x kubectl```
Moveremos el archivo a los binarios
```sudo mv kubectl /usr/local/bin```

# Instalacion de miniKube

<https://kubernetes.io/docs/tasks/tools/install-minikube/>

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

<https://docs.docker.com/engine/reference/commandline/docker/>

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
- ```docker stop name```: parar un contenedor ya creado y arrancado
- ```docker rm name -f```: eliminar un contenedor que esta corriendo
- ```docker create --name web01 helloworld:latest``` : crear un container con nombre
- ```docker inspect web01```: Inspeccionar contenedor
- ```sudo docker logs xxx```: Para ver los errores de un docker

## Creando una imagen

```Dockerfile```

```Dockerfile
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

```docker inspect NAME|grep IPAddress```

## crear un contenedor publicando un puerto especifico

```docker create -p 8080:80 --name web01 helloworld:latest```

## crear un contenedor publicando un puerto especifico en un interfaz concreto

```docker create -p ip:8080:80 --name web01 helloworld:latest```

## crear la nueva imagen

```PowerShell
docker build -t helloworld:2.0
```

## taguear como latest

```PowerShell
docker tag helloworld:2.0 helloworld:latest
```

## crear un contenedor publicando a un puerto local expuesto en la imagen

```PowerShell
docker create -P --name web01 helloworld:latest
```

## Crear contenedor y arrancarlo a la vez en segundo plano con un solo comando

```PowerShell
docker run -d ...
```

## Comprobar rango de puertos locales

```PowerShell
cat /proc/sys/net/ipv4/ip_local_port_range
```

## Instalar terminator

```PowerShell
sudo apt install terminator -y
```

## Crear nueva division horizontal en terminator

```PowerShell
Ctrl + Shift + o
```

## crear contenedor

```PowerShell
docker run --name web01 -d -P helloworld
```

## Algunos ejemplos de ejecutar comandos dentro del contenedor con exec

```PowerShell
docker exec web01 ls /
docker exec web01 ps
```

## Ejecutar la shell bash dentro de un contenedor que esta corriendo

```PowerShell
docker exec -it web01 bash
```

## Ver logs de nginx dentro del contenedor

```PowerShell
tail -f /var/log/nginx/access.log
```

## listar volumenes

```PowerShell
docker volume ls
```

## Crear volumen

```PowerShell
docker volume create my-volume
```

## Inspeccionar volumen

```PowerShell
docker volume inspect my-volume
```

## crear contenedor con volumen persistente

```PowerShell
docker create -P --mount source=my-volume,target=/var/www/html --name web01 helloworld:latest
```

## eliminar un volumen

```PowerShell
docker volume rm my-volume
```

## limpiar o purgar el listado de volumenes no utilizados

```PowerShell
docker volume prune
```

## listar redes

```PowerShell
docker network ls
```

## Crear network

```PowerShell
docker network create nombre_de_la_red --driver nombre_del_driver 
por ejemplo:
docker network create web --driver bridge
```

## Crear un contenedor dentro de una red

```PowerShell
docker create --name web01 --network web helloworld
```

## filtrar ip en la salida de docker inspect

```PowerShell
docker inspect web01 |grep IPAddress
```

## eliminar las redes que no esten en uso

`docker network prune`

## conectar y desconectar un contenedor de una red

```PowerShell
docker connect web web01
docker disconnect web web01
```

## docker login

```docker login```

## etiquetar imagen

```docker tag imagenlocal:etiqueta usuarioremoto/imagenremota:etiqueta```

## subir imagen

```docker push usuarioremoto/imagenremota:etiqueta```

## descargar imagen

```docker pull usuarioremoto/imagen:etiqueta```

## eliminar todas las imagenes

```docker rmi -f $(docker images -q)```

## eliminat todos los contenedores

```docker rm $(docker ps -a -q)```

## Leer variables de entorno

``` env ```

## leer contenido de la variable de entorno SHELL

``` echo $SHELL ```

## crear contenedor inyectando variables de entorno

```docker create -e variable1=valor1 -e variable2=valor2 imagen:tag```

## siempre tagear las imagaenes

```PowerShell
docker tag helloworld:4.0 helloworld:latest
docker tag helloworld:4.0 anthonysnk/helloworld:4.0
docker tag helloworld:4.0 anthonysnk/helloworld:latest
```

# Docker compose

Con esta herramientas podemos ejecutar entornos multi contenedor.
<https://docs.docker.com/compose/>

## Levantar contenedor o contenedores definidos con docker compose en primer plano

```docker-compose up```

## Levantar contenedor o contenedores definidos con docker compose en segundo plano

```docker-compose up -d```

## Parar contenedores definidos con docker compose

```docker-compose down```

## Eliminar contendores creados con docker compose

```docker-compose rm```

## ver los logs

```docker-compose logs -f```

## Construir imagen a traves de docker-compose

```docker-compose build```

# Pasarle archivos al docker

En caso de el nginx  es con el parametro -v

```PowerShell
docker run -d -p 80:80 --name website -v $(psw):/usr/share/nginx/html
# Laboratorio
```

## Parte 1

```docker pull mariadb```

## Crear network

```docker network create lab```

## Crear contenedor de pruebas con mariadb:latest1

```docker run --net lab --name db01 -e MYSQL_ROOT_PASSWORD=password -d mariadb:latest```

## Conectarnos al contenedor y comprobar la variable de entorno

```PowerShell
docker exec -it db01 bash
env
```

### Conectarnos a mysql y listar bases de datos

```PowerShell
mysql -uroot -ppassw0rd
show databases;
```

### conectar a la base de datos desde otro contenedor y listar bases de datos

```PowerShell
docker run -it --name test --network lab --rm mariadb:latest mysql -hdb01 -uroot -p
show databases;
```

### Importamos la base de datos

```docker exec -i db01 sh -c 'exec mysql -uroot -p"$MYSQL_ROOT_PASSWORD"' < dump/users.sql```

### conectar a la base de datos desde otro contenedor y listar los cambios

```PowerShell
docker run -it --name test --network lab --rm mariadb:latest mysql -hdb01 -uroot -p
show databases;
use users;
show tables;
select * from users;
```

## Parte 2

## Crear la imagen de nuestra aplicacion

```PowerShell
cd app
docker build -t python . 
```

## Ejecutar nuestra imagen

```PowerShell
docker run --network lab -e db_host=db01 -e db_user=root -e MYSQL_ROOT_PASSWORD=password -e db_name=users --name python --rm -p 5000:5000 python:latest
```

## Conectamos a la base de datos e insertamos nuevos datos

```PowerShell
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

- `kubectl get nodes`: Devuelve los nodos del cluster
- `minikube status`: Devuelve el stado del host, kubelet, apiserver, kubeconfig
- `minikube pause`: Este comando pausara el cluster (Similar a hivernar la maquina)
- `minikube unpause`: restaurar el cluster a su forma no detenida
- `minikube stop`: Detiene por completo el cluster y sus componentes (similiar como apagar la maquina virtual)
- `minikube start`: Inicia el cluster y sus componentes
- `minikube delete`: Borrara los cluster
- `minikube config view`:  muestra la configacion del archivo ./minikube/config/config.json
- `minikube config get cpus`: Devolvera los cpu del archivo de configuraciones, puede utilizarse para cualquier parametro
- `minikube config unset cpus`: Eliminara los opcion que le pasemos del archivo de configuracion

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

- `minikube ip`:obteniendo la ip del cluster
- `minikube logs`: Logs del nuestro sistema de los diferentes contedores
- `minikube logs -f --problems=true`: mostrara los los errores
- `minikube update-check`: muestra la version actual y cual es la version nueva
- `minikube ssh`: para conectarnos a la maquina virtual
- `ps aux`: para observar que esta corriendo dentro de la maquina
- `docker ps`: para ver que contenedores tiene ma laquina virtual
- `tail -f /var/log/`: para ver los logs
- `docker exec -it ID bash`: para ver que esta corriendo dentro de los contenedores

## Plugins de minikube (addons)

No permitiran instalar ciertos componentes.

- `minikube addons list`: nos muestran los plugins
- `minikube addons enable NombrePlugin`: Para poder activar un plugin
- `minikube addons enable dashboard`: para activar el addon de dashboard
- `minikube dashboard`: para lanzar el addon

# Kubectl

Es una herramienta de linea de comando que va a permitir controla uno o mas cluster.
La sintaxis de los comando es
`kubectl <commad> <recurso> <nombre> <argumento>`

Los comando suele ser un verbo que no indica una accion como lo son: `create`, `expose`, `run`, `set`, etc

## comandos

- `kubectl get nodes`: Entrega los nodos de nuestro cluster
- `kubectl describe node minikube`: para obtener solo el nodo que le indiquemos
- `kubectl get node -o wide`: Nos devuelve una salida mas completa de la salida de los nodos
- `kubectl get node -o json`: Nos muestra una salida en formato json de la informacion de los recursos
- `kubectl describe nodes`: Muestra toda la informacion de los nodos
- `kubectl get all`:Devuelve todos los recursos
- `kubectl apply -f ejemplo.yaml`: creando manifests
- `kubectl delete -f ejemplo.yaml`: Elimina el manisfests indicado
- `kubectl apply -f pod1.yaml` : Aplicando el manifest del pod
- `kubectl get pods` o `kubectl get pod`: Devuelve los pods
- `kubectl get pods -o wide`: devuele mas informacion de pods
- `kubectl describe pods`: describir la informacion del pod
- `kubectl describe pod nginx`: descriibe la informacion del pod deseado
- `kubectl delete pod nginx`: borra un el pod especificado
- `kubectl delete -f pod1.yaml`: borrar el pod desde el manisfest
- `kubectl get pod --show-labels`: muestra las etiquetas del pod

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

### Comandos Con manifest

```PowerShell
kubectl apply -f ejemplo.yaml
kubectl get all
kubectl delete -f ejemplo.yaml
```

### Comandos Sin manifest

```PowerShell
kubectl run nginx --image=nginx:latest --port 80 --replicas=1
kubectl get all
kubectl delete deployment nginx
```

## Pods

Es el objeto minimo para desplegar en kubernetes
El pods siempre incluira un contenedor
Un pods se inicia y es mas que un contenedor donde correra un cmd, puede correrse hasta que se le ordene parar.

NOTA: No pueden existir dos pods que se llamen igual

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

## comandos selectors

Los selectors nos ayudan referirnos a un recurso mediante sus etiquetas

```Powershell
kubectl apply -f pod.yaml -f metadata.yaml # instala el pod
kubectl get pods # obtenemos los pods creados
kubectl get pods -w 
kubectl get pod --show-labels # muestra los etiquetas de los pods
kubectl get pod --show-labels --selector environment 
kubectl get pod --show-labels --selector environment=testing # seleccionamos el ambiente testing
kubectl get pod --show-labels --selector environment=production
kubectl get pod --show-labels --selector environment
kubectl get pod --show-labels --selector proyecto
kubectl get pod --show-labels --selector project
kubectl get pod --show-labels --selector project=nginx
kubectl get pod --show-labels --selector nginx
kubectl get pod --show-labels --selector testing
kubectl delete -f metadata.yaml -f pod.yaml # eliminando los pods
```

# Replication controlers

Es similar a un autoScalingGroup

```Powershell
kubectl apply -f pod-replicationcontroller.yaml 
kubectl get all
kubectl delete pod nginx-testing-xxxxx
kubectl get pods,replicationcontroller
kubectl get pods,rc
kubectl describe rc
kubectl describe rc nginx
kubectl delete -f pod-replicationcontroller.yaml 
```

# replicaset

Similares a replicationcontroller y el sucesos de estos

```PowerShell
kubectl apply -f replicaset.yaml 
kubectl get all
kubectl delete -f replicaset.yaml 
```

# Servicios

Entre los servicios que podesmos asignar estan:

- ClusterIP:

  expone el servicio en una IP interna del clúster. La elección de este valor hace que el servicio solo sea accesible desde dentro del clúster. Este es el ServiceType predeterminado

- NodePort

  expone el servicio en la IP de cada nodo en un puerto estático (el NodePort). Se crea automáticamente un servicio ClusterIP, al que se enrutará el servicio NodePort. Podrá ponerse en contacto con el servicio NodePort, desde fuera del clúster, solicitando

  ```html
  <NodeIP>:<NodePort>
  ```

- LoadBalancer

  expone el servicio de forma externa mediante el equilibrador de carga de un proveedor de nube. Los servicios NodePort y ClusterIP, a los que se enrutará el equilibrador de carga externo, se crean automáticamente

- ExternalName

  Nos permite crear un aliasDNS que solo aplicara dentro del cluster

```yaml
apiVersion: v1
kind: Service
metadata:
  name: servicio1
spec:
  type: ClusterIP
  selector:
    app: nginx
    environment: testing
  ports:
    - protocol: TCP
      port: 80
```

# Persisten volume

NOTA: No se puede eliminar un volumen si esta atachado

- `kubectl get pv` o `kubectl get persistentvolume`: En lista los volumenes persistentes existentes
- `kubectl get pvc` : utilizado para ver los los persistent volume clain (Los reclamos a un volumen)

# ConfigMaps

Son valores de configuracion que se pasan a los contenedores al momento de la creacion.

- `kubectl get cm` o `kubectl get configmap`: En listar los configmaps que poseamos.
- `kubectl delete cm Nombre`: eliminar el configmap
  
## Creando un configMap desde la linea de comandos

`kubectl create configmap test-m --from-literal variable1=valor1` Donde `--from-literal` hace referencia a que se ingresara otra variable.

`kubectl create cm nginx-conf-dir --from-file=./nginx-config-map`: creando un cm desde un archivo.

```conf
server {
    listen       80;
    server_name  videocursoscloud.com;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
}
```

NOTA: Es recomendable obtener el manifest atraves del comando ya que es muy complejo

`kubectl create cm nginx-conf-dir --from-file=./nginx-config-map --dry-run -o yaml`

`kubectl create cm nginx-conf-dir --from-file=./nginx-config-map --dry-run=client -o yaml`

`kubectl create cm nginx-conf-dir --from-file=./nginx-config-map --dry-run=server -o yaml`

En este caso los ficheros o directorios se toman como volumenes eg

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-cm-file
spec:
  containers:
    - name: nginx
      image: nginx
      volumeMounts:
        - name: config-volume
          mountPath: /etc/nginx/conf.d/
  volumes:
    - name: config-volume
      configMap:
        name: nginx-config-dir
```

# Secrets

Son similares que los configmap solamente que iran encriptados en los servidores
Se utilizaran cuanod necesitamos almacenar claves ssh, password, api etc.

## comandos

- `kubectl create secret generic credenciales --from-file=./username --from-file=./password`: donde `credenciales` es el nombre que le asiganmos al secret y los archivos son en donde tenemos el username y el passwrod
- `kubectl get secrets`: para en listar los secret que tenemos
- `kubectl create secret generic credenciales --from-file=./username --from-file=./password -o yaml` Para obtener el archivo, para poder crear el secret atraves de un manisfest
- `kubectl describe secret NOMBRE_DEL_SECRET`:  para poder obtener la descripcion del secret, como los campos que tienen
- `kubectl get secret NOMBRE_DEL_SECRET -o yaml`: para obtener los campos y sus value

## tipos

```link
https://kubernetes.io/docs/concepts/configuration/secret/#:~:text=Kubernetes%20provides%20a%20builtin%20Secret,or%20directly%20by%20a%20workload.
```

 | Builtin Type                        | Usage                                 |
 | ----------------------------------- | ------------------------------------- |
 | Opaque                              | arbitrary user-defined data           |
 | kubernetes.io/service-account-token | service account token                 |
 | kubernetes.io/dockercfg             | serialized ~/. dockercfg file         |
 | kubernetes.io/dockerconfigjson      | serialized ~/.docker/config.json file |
 | kubernetes.io/basic-auth            | credentials for basic authentication  |
 | kubernetes.io/ssh-auth              | credentials for SSH authentication    |
 | kubernetes.io/tls                   | data for a TLS client or server       |
 | bootstrap.kubernetes.io/token       | bootstrap token data                  |

# Deployments

Permiten desplegar la app para hacer roolout  y poder volver a versiones a nateriores si fuese necesario, gestionan los replicasets. Un controlador de Deployment proporciona actualizaciones declarativas para los Pods y los ReplicaSets.

Podemos tener hasta un maximo de 10 deployment para poder regresar a esas versiones


## Comandos

### Available Commands for rolout

- `kubectl rollout command`

  | command | use                                  |
  | ------- | ------------------------------------ |
  | history | View rollout history                 |
  | pause   | Mark the provided resource as paused |
  | restart | Restart a resource                   |
  | resume  | Resume a paused resource             |
  | status  | Show the status of the rollout       |
  | undo    | Undo a previous rollout              |

### desplegando una imagen

`kubectl set image deployment nginx-deployment nginx=1.15`: se creara un replicaset nuevo.

### Volver a una version anterior

`kubectl rollout history desployment NOMBRE_DEPLOY`: se va a la version anterior

`kubectl rollout undo deployment NOMBRE_DEPLOY --to-revision=1`: donde el numero es el numero de la revision que podemos verlo con el commando `history`

`kubectl rollout pause deployment NOMBRE_DEPLOY`: No permitira volver a versiones anteriores antes que apliquemos el resume

# Namespaces

Permite organizar los recursos en diferente espacios de nombre, permitiendo aislar los recursos,util para poder acceso a users a determinados namespaces.

## Comandos

| comando                                      | use                                                      |
| -------------------------------------------- | -------------------------------------------------------- |
| `kubectl get namespaces`                     | En listar los namespaces                                 |
| `kubectl get all --name NOMBRESPACE`         | En lista los componentes de ese namespace                |
| `kubeclt get pods -A`                        | Lista los pods de todos los namespace                    |
| `kubeclt get svc -A`                         | Lista los servicios de todos los namespace               |
| `kubectl api-resources`                      | Lista los recursos para ver si va dentro de un namespace |
| `kubectl -api-resources --namespaced=true`   | Muestra la lista filtrada                                |
| `kubectl create ns prueba --dry-run -o yaml` | Para simular la creacion y obtener el yaml               |
| `kubectl -n prueba get pods`                 | Una forma mas productiva de listar                       |
| `kubectl delete ns NOMBRE`                   | Eliminara el namespace con todo lo que este dentro de el |

Nota: No todos los recursos se crean en un namespaces ejemplos los `volume` estos se crean a nivel de cluster

## Creacion de un namespace

`kubectl create ns NOMBRESPACE`

## Crear un pot dentro del namespace

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx2
spec:
  containers:
  - name: nginx
    image: nginx:1.7.9
    ports:
    - containerPort: 80
```

# Kubernetes en AWS con Kops

```links
https://kubernetes.io/docs/setup/production-environment/tools/kops/
https://github.com/kubernetes/kops/tree/master/docs
```

## Instalacion en Linux

`curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64`

`sudo mv kops-linux-amd64 /usr/local/bin/kops`

## Comandos

- `kops create cluster --name testkops.snkops.com --zones=us-east-1a,us-east-1b,us-east-1c --master-zones=us-east-1a,us-east-1b,us-east-1c --cloud aws --ssh-public-key ~/.ssh/id_rsa.pub` : crearemos un cluster con zonas de disponibilidad en aws, le pasaremos una llave publica de nuestro equipo tambien. NOTA:`necesitamos crear unaa zona en route53`

vars.rc

```bash
export KOPS_STATE_STORE="s3://vcc-kops-test-snk/"
```

- `source vars.rc`: con esta exportamos nuestra varible KOPS_STATE_STORE al env.