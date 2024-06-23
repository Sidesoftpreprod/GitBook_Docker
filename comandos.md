---
description: Detalle de comandos utiles
---

# Comandos

#### Tamaño que ocupa Docker

> docker system df -v

#### Exportar imagen

> docker save myimage:latest | gzip > myimage\_latest.tar.gz

#### Importar imagen

> docker load < busybox.tar.gz

#### Copiar del host al contenedor

> docker cp ./some\_file CONTAINER:/work

#### Copiar del contenedor al host

> docker cp CONTAINER:/var/logs/ /tmp/app\_logs

**Detener todos los contenedores**

> docker stop $(docker ps -q)

```
Remove all the containers
```

```
docker rm $(docker ps -a -q)
```
