# 20201 - Sistemas Distribuidos - Docker Workshop 

## Practica 1 - Comandos Docker

### 1) Consultar la version de docker
```terminal
docker version
```
```
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
```terminal
docker run hello-world
```
```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

### 3) Consultar los contenedores en ejecucion
```terminal
docker ps
```
```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```
En este caso, no hay contenedores en ejecucion.

Para ver los contenedores, incluyendo los que ya se no se están ejecutando
```terminal
docker ps -a
```
```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                       PORTS                NAMES
c2b02987dc5a        hello-world         "/hello"            About a minute ago   Exited (0) About a minute ago                       mystifying_panini
```

### 4) Eliminar un contenedor ya apagado
Es necesario ID de Contenedor (CONTAINER ID) o el nombre del contenedor (NAMES) para poder seleccionarlo y eliminarlo de la lista.

Para eliminarlo por **CONTAINER ID**:
```terminal
docker rm c2b02987dc5a
```
```
c2b02987dc5a
```

Para eliminarlo por **NAMES**:
```terminal
docker rm mystifying_panini
```
```
mystifying_panini
```

### 5) Ejecutar un contenedor permanentes (servidor demonio)
Un servidor permanente es aquel que siempre va a estar en ejecución dentro de la máquina, escuchando por un puerto y respondiendo a peticiones de clientes.

Aqui usaremos de nuevo el comando **run** pero con unos nuevos ajustes:
* `-d`: Ejecuta el contenedor en segundo plano (demonio)
* `--name`: Asigna el nombre del contenedor.
* `-p`: realiza un mapeo entre el puerto del contenedor al puerto en el host. 
```terminal
docker run -d --name web -p 80:80 nginx
```
```
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

```terminal
docker ps
```
```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                NAMES
8c1d70cdf9f2        nginx               "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp   web
```

### 6) Ingresar al shell del sistema operativo
```terminal
docker exec -it web /bin/bash
```
```terminal
root@8c1d70cdf9f2:/# ls -l /
```
```
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
```
```terminal
root@8c1d70cdf9f2:/# exit
```

### 7) Parar un contenedor permanente
```terminal
docker stop 8c1d70cdf9f2
```
```
8c1d70cdf9f2
```

```terminal
docker stop web
```
```
web
```

### 8) Obtener ayuda de los comandos de docker
```terminal
docker help
```
```
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

## Practica 3 - Publicando imagen en Docker Hub

## Practica 4 - Desplegando aplicación con Docker Compose

# Para seguir estudiando...
1. [Curso de Docker](https://www.youtube.com/watch?v=UZpyvK6UGFo&list=PLqRCtm0kbeHAep1hc7yW-EZQoAJqSTgD-)
1. [Katacoda - Learn Docker, Container Runtimes, Builders and Registries using Interactive Browser-Based Scenarios](https://www.katacoda.com/courses/container-runtimes)
1. [Docker 101 Tutorial](https://www.docker.com/101-tutorial)
1. [Docker for beginners](https://docker-curriculum.com/)
1. [Aprendiendo a utilizar Docker Compose](https://dockertips.com/utilizando-docker-compose)
