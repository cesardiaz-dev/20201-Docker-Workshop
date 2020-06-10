# 20201 - Sistemas Distribuidos - Docker Workshop 

## Practica 1 - Comandos Docker

### 1) Consultar la version de docker
```bash
> docker version

Client: Docker Engine - Community
 Version:           19.03.8
 API version:       1.40
 Go version:        go1.12.17
 Git commit:        afacb8b
 Built:             Wed Mar 11 01:23:10 2020
 OS/Arch:           windows/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.8
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.17
  Git commit:       afacb8b
  Built:            Wed Mar 11 01:29:16 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
 Kubernetes:
  Version:          v1.16.6-beta.0
  StackAPI:         v1beta2
```

### 2) Ejecutar el contenedor "Hola Mundo!"
```bash
> docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your bash.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

### 3) Consultar los contenedores en ejecucion
```bash
> docker ps
 
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```
En este caso, no hay contenedores en ejecucion.

Para ver los contenedores, incluyendo los que ya se no se están ejecutando
```bash
> docker ps -a

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                       PORTS                NAMES
c2b02987dc5a        hello-world         "/hello"            About a minute ago   Exited (0) About a minute ago                       mystifying_panini
```

### 4) Eliminar un contenedor ya apagado
Es necesario ID de Contenedor (CONTAINER ID) o el nombre del contenedor (NAMES) para poder seleccionarlo y eliminarlo de la lista.

Para eliminarlo por **CONTAINER ID**:
```bash
> docker rm c2b02987dc5a

c2b02987dc5a
```

Para eliminarlo por **NAMES**:
```bash
> docker rm mystifying_panini

mystifying_panini
```

### 5) Ejecutar un contenedor permanentes (servidor demonio)
Un servidor permanente es aquel que siempre va a estar en ejecución dentro de la máquina, escuchando por un puerto y respondiendo a peticiones de clientes.

Aqui usaremos de nuevo el comando **run** pero con unos nuevos ajustes:
* `-d`: Ejecuta el contenedor en segundo plano (demonio)
* `--name`: Asigna el nombre del contenedor.
* `-p`: realiza un mapeo entre el puerto del contenedor al puerto en el host. 
```bash
> docker run -d --name web -p 80:80 nginx

Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
afb6ec6fdc1c: Pull complete
dd3ac8106a0b: Pull complete
8de28bdda69b: Pull complete
a2c431ac2669: Pull complete
e070d03fd1b5: Pull complete
Digest: sha256:15c65919f2b5889636c671b99ca4e70eff1e78c3114d60600b29286e476e6876
Status: Downloaded newer image for nginx:latest
8c1d70cdf9f2e6a1f93073734189de86ae2eb722f2965b3e88935352d1c40bd7
```

```bash
> docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                NAMES
8c1d70cdf9f2        nginx               "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp   web
```

### 6) Ingresar al shell del sistema operativo
```bash
> docker exec -it web /bin/bash

root@8c1d70cdf9f2:/# ls -l /

total 80
drwxr-xr-x   2 root root 4096 May 14 14:50 bin
drwxr-xr-x   2 root root 4096 May  2 16:39 boot
drwxr-xr-x   5 root root  340 Jun  9 06:21 dev
drwxr-xr-x   1 root root 4096 Jun  2 16:23 docker-entrypoint.d
-rwxrwxr-x   1 root root 1087 Jun  2 00:35 docker-entrypoint.sh
drwxr-xr-x   1 root root 4096 Jun  9 06:21 etc
drwxr-xr-x   2 root root 4096 May  2 16:39 home
drwxr-xr-x   1 root root 4096 Jun  2 00:35 lib
drwxr-xr-x   2 root root 4096 May 14 14:50 lib64
drwxr-xr-x   2 root root 4096 May 14 14:50 media
drwxr-xr-x   2 root root 4096 May 14 14:50 mnt
drwxr-xr-x   2 root root 4096 May 14 14:50 opt
dr-xr-xr-x 172 root root    0 Jun  9 06:21 proc
drwx------   1 root root 4096 Jun  9 06:22 root
drwxr-xr-x   1 root root 4096 Jun  9 06:21 run
drwxr-xr-x   2 root root 4096 May 14 14:50 sbin
drwxr-xr-x   2 root root 4096 May 14 14:50 srv
dr-xr-xr-x  12 root root    0 Jun  9 06:21 sys
drwxrwxrwt   1 root root 4096 Jun  2 00:35 tmp
drwxr-xr-x   1 root root 4096 May 14 14:50 usr
drwxr-xr-x   1 root root 4096 May 14 14:50 var

root@8c1d70cdf9f2:/# exit

>
```

### 7) Parar un contenedor permanente
```bash
> docker stop 8c1d70cdf9f2

8c1d70cdf9f2

> docker stop web

web
```

### 8) Obtener ayuda de los comandos de docker
```bash
> docker help

Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default
                           "C:\\Users\\DiazOspina\\.docker")
  -c, --context string     Name of the context to use to connect to the
                           daemon (overrides DOCKER_HOST env var and
                           default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level
                           ("debug"|"info"|"warn"|"error"|"fatal")
                           (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default
                           "C:\\Users\\DiazOspina\\.docker\\ca.pem")
      --tlscert string     Path to TLS certificate file (default
                           "C:\\Users\\DiazOspina\\.docker\\cert.pem")
      --tlskey string      Path to TLS key file (default
                           "C:\\Users\\DiazOspina\\.docker\\key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  builder     Manage builds
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.
```

## Practica 2 - Creando imagen basico
En esta practica vamos a crear una miniaplicacion web y publicada automaticamente en un contenedor
### 1) Crear el archivo **index.html**
```html
<h1>Hello World</h1>
```

### 2) Crear el archivo **Dockerfile**
```Dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
```

###  3) Construir la imagen del proyecto
Construir la imagen de nuestra aplicación. Utilizar como agrupador el nombre de usuario que se utilizó en el registro en [Docker Hub](https://hub.docker.com/signup), en mi caso es **negro2k2**
```bash
> docker build -t negro2k2/webserver-image:v1 .

Sending build context to Docker daemon  3.072kB
Step 1/2 : FROM nginx:alpine
 ---> 29b49a39bc47
Step 2/2 : COPY . /usr/share/nginx/html
 ---> b01197432c36
Successfully built b01197432c36
Successfully tagged negro2k2/webserver-image:v1
SECURITY WARNING: You are building a Docker image from Windows against a non-Windows Docker host. All files and directories added to build context will have '-rwxr-xr-x' permissions. It is recommended to double check and reset permissions for sensitive files and directories.
```

Consultar las imagenes que se encuentran en nuestro sistema (descargadas y construidas)
```bash
> docker images

REPOSITORY                  TAG                 IMAGE ID            CREATED              SIZE
negro2k2/webserver-image    v1                  b01197432c36        About a minute ago   21.2MB
nginx                       alpine              7d0cdcc60a96        6 days ago           21.2MB
redis                       latest              4760dc956b2d        2 years ago          107MB
ubuntu                      latest              f975c5035748        2 years ago          112MB
alpine                      latest              3fd9065eaf02        2 years ago          4.14MB
```

Ejecutar el contenedor a partir de la imagen construida
```bash
> docker run -d -p 80:80 negro2k2/webserver-image:v1

f5240daeea6695fc9a33c414d973498c85a309360b45a65c25f908a3da5ffa8b
```

Ahora podemos probar que la aplicacion ha sido desplegada navegando al [host local](http://localhost)

### 4) Publicando imagen en Docker Hub. _Basado en [Docker Hub Quickstart](https://docs.docker.com/docker-hub/)_

1. Inicia sesión en [Docker Hub](https://hub.docker.com/signup)

2. Crear un **repositorio** en Docker Hub con el nombre `webserver-image`

3. Construir(si es necesario) la imagen en su computador: `docker build -t negro2k2/webserver-image:v1  .`

4. Iniciar sesión (si no lo ha echo anteriormente) en docker por la consola con `docker login`

5. Publicar la imagen en Docker Hub: `docker push negro2k2/webserver-image:v1`
```
The push refers to repository [docker.io/negro2k2/webserver-image]
48285129616d: Pushed
debb5485a52f: Mounted from negro2k2/docker101tutorial
beee9f30bc1f: Mounted from negro2k2/docker101tutorial
v1: digest: sha256:708b57b45928e69b582c2048e19b950b4d3b7dd86db4bc520b29f94fea69b8c5 size: 946
```

### 5) Eliminar la imagen local

#### a. Eliminación exitosa
```bash
> docker rmi b01197432c36

Untagged: negro2k2/webserver-image:v1
Untagged: negro2k2/webserver-image@sha256:708b57b45928e69b582c2048e19b950b4d3b7dd86db4bc520b29f94fea69b8c5
Deleted: sha256:b01197432c367f9039180d463365081b3a0ef7d6c54319744e9b94cf4bf4d9e2
Deleted: sha256:28ef4c066a326e9ff2460b58bbfb6c598573ab473315d05ad7b71c8b41b32e64
```

#### b. Eliminación no exitosa
En el caso que aun se encuentre en ejecución el contenedor o esté dentro de la lista de contenedores creados, la imagen no se deja eliminar, entonces:

```bash
> docker rmi b01197432c36
Error response from daemon: conflict: unable to delete b01197432c36 (cannot be forced) - image is being used by running container e1a52d1ec6b0

> docker ps -a
CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS                      PORTS                NAMES
e1a52d1ec6b0        negro2k2/webserver-image:v1   "nginx -g 'daemon of…"   22 seconds ago      Up 17 seconds               0.0.0.0:80->80/tcp   busy_kepler
0090b434372f        negro2k2/webserver-image:v1   "nginx -g 'daemon of…"   22 minutes ago      Exited (0) 34 seconds ago                        admiring_banach
7c6a5cce22f0        nginx                         "/docker-entrypoint.…"   9 hours ago         Exited (0) 8 hours ago                           web
```

1. Es necesario parar el contenedor, eliminar los contenedores creados de dicha imagen 
```bash
> docker stop busy_kepler
busy_kepler

> docker rm busy_kepler
busy_kepler

> docker rm admiring_banach
admiring_banach

> docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                   PORTS               NAMES
7c6a5cce22f0        nginx               "/docker-entrypoint.…"   9 hours ago         Exited (0) 8 hours ago                       web
```

2. Eliminar la imagen
```bash
> docker rmi b01197432c36
Untagged: negro2k2/webserver-image:v1
Untagged: negro2k2/webserver-image@sha256:708b57b45928e69b582c2048e19b950b4d3b7dd86db4bc520b29f94fea69b8c5
Deleted: sha256:b01197432c367f9039180d463365081b3a0ef7d6c54319744e9b94cf4bf4d9e2
Deleted: sha256:28ef4c066a326e9ff2460b58bbfb6c598573ab473315d05ad7b71c8b41b32e64
```

### 6) Ahora puedes descargar a tu computador (o en cualquier otro Docker host) su aplicacion publicada

```bash
> docker run -d -p 80:80 negro2k2/webserver-image:v1

Unable to find image 'negro2k2/webserver-image:v1' locally
v1: Pulling from negro2k2/webserver-image
aad63a933944: Already exists
b14da7a62044: Already exists
4fcc19494f67: Pull complete
Digest: sha256:708b57b45928e69b582c2048e19b950b4d3b7dd86db4bc520b29f94fea69b8c5
Status: Downloaded newer image for negro2k2/webserver-image:v1
0090b434372f2910e32efe3e9e9cb50922f615238df469fc2795686c2581cfd0
```

###  **Tarea**:  Hacer una modificación en la pagina web y desplegar la actualización con una nueva versión

## Practica 3 - Desplegando aplicación con Docker Compose. _Basado en **[Como levantar un servicio web con docker y docker-compose](https://blog.dinahosting.com/servicio-web-con-docker-y-docker-compose/)**_
Con docker, se suele empaquetar un solo servicio por contenedor, pero, cuando la solución a desplegar necesita mas de un contenedor, se necesita un orquestador, es decir, una herramienta que permita manejar en conjunto varios contenedores relacionados entre sí. Para este fin, se usa **Docker Compose**.

Para poder levantar los servicios que son necesarios, hay que indicarle a Docker Compose lo siguiente:
* Los contenedores a crear/levantar
* La relación de los contenedores entre sí

Para ello creamos un fichero **YAML**, llamado _**docker-compose.yml**_, donde se define cada uno de los servicios

```yaml
version: "3"
 
services:
  miservicio_mysql:
    image: mysql:5.7
    environment:
      - MYSQL_DATABASE=nombre
      - MYSQL_ROOT_PASSWORD=claveroot
      - MYSQL_USER=miusuario
      - MYSQL_PASSWORD=mipassword
    volumes:
      # Montamos un volumen para MySQL para no perder los datos de bd
      - ./mysql:/var/lib/mysql
    expose:
      - 3306
    ports:
      - 3306:3306
   
  miservicio_php:
    image: php:7-apache
    volumes:
      # Montamos nuestra web desde fuera en el directorio web del contenedor
      - ./web:/var/www/html
    expose:
      - 80
    ports:
      - 80:80
    links: 
      - miservicio_mysql
```

1. Iniciar el despliegue los contenedores
```bash
> docker-compose up -d

Creating network "practica3_default" with the default driver
Pulling miservicio_mysql (mysql:5.7)...
5.7: Pulling from library/mysql
8559a31e96f4: Pull complete
d51ce1c2e575: Pull complete
c2344adc4858: Pull complete
fcf3ceff18fc: Pull complete
16da0c38dc5b: Pull complete
b905d1797e97: Pull complete
4b50d1c6b05c: Pull complete
d85174a87144: Pull complete
a4ad33703fa8: Pull complete
f7a5433ce20d: Pull complete
3dcd2a278b4a: Pull complete
Digest: sha256:32f9d9a069f7a735e28fd44ea944d53c61f990ba71460c5c183e610854ca4854
Status: Downloaded newer image for mysql:5.7
Pulling miservicio_php (php:7-apache)...
7-apache: Pulling from library/php
8559a31e96f4: Already exists
e0276193a084: Pull complete
eb2d00c10344: Pull complete
f54006e0dc29: Pull complete
e0d3d1244592: Pull complete
3a60f364b0c5: Pull complete
3e309988c00b: Pull complete
5d92b4c08548: Pull complete
83faa9e6981c: Pull complete
8f4f490c1749: Pull complete
e75eda9c13c2: Pull complete
af5ca20c6fe0: Pull complete
ecf22074a2a4: Pull complete
Digest: sha256:8b741fab33229187b2bc161b2176ba07997664d00a778c0a7df59514440f0acc
Status: Downloaded newer image for php:7-apache
Creating practica3_miservicio_mysql_1 ... done
Creating practica3_miservicio_php_1   ... done
```

2. Verificar el estado de los contenedores del proyecto actual
```bash
> docker-compose ps -a

            Name                          Command               State                 Ports
---------------------------------------------------------------------------------------------------------
practica3_miservicio_mysql_1   docker-entrypoint.sh mysqld      Up      0.0.0.0:3306->3306/tcp, 33060/tcp
practica3_miservicio_php_1     docker-php-entrypoint apac ...   Up      0.0.0.0:80->80/tcp
```

3. Parar los contenedores en ejecucion
```bash
> docker-compose stop

Stopping practica3_miservicio_php_1   ... done
Stopping practica3_miservicio_mysql_1 ... done
```

4. Eliminar los contenedores desplegados por esta aplicación
```bash
> docker-compose down

Removing practica3_miservicio_php_1   ... done
Removing practica3_miservicio_mysql_1 ... done
Removing network practica3_default
```

# Para seguir estudiando...

## docker
1. [Curso de Docker](https://www.youtube.com/watch?v=UZpyvK6UGFo&list=PLqRCtm0kbeHAep1hc7yW-EZQoAJqSTgD-) (Youtube)
1. [Play with Docker Classroom](https://training.play-with-docker.com/) (Recomendado)
1. [Katacoda - Learn Docker, Container Runtimes, Builders and Registries using Interactive Browser-Based Scenarios](https://www.katacoda.com/courses/container-runtimes)
1. [Docker 101 Tutorial](https://www.docker.com/101-tutorial)
1. [Docker for beginners](https://docker-curriculum.com/)
1. [Linea de comandos - docker](https://docs.docker.com/engine/reference/commandline/cli/)
1. [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)


## docker-compose
1. [Aprendiendo a utilizar Docker Compose](https://dockertips.com/utilizando-docker-compose)
1. [Ejecutar aplicaciones multi-contenedor con docker-compose](https://www.returngis.net/2019/02/ejecutar-aplicaciones-multi-contenedor-con-docker-compose/) (Recomendado)
1. [Linea de comandos - docker-compose](https://docs.docker.com/compose/reference/overview/)
1. [Compose file reference](https://docs.docker.com/compose/compose-file/)
