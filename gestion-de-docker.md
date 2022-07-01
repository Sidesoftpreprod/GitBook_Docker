# Gestión de Docker

Docker dispone de un [CLI ](https://docs.docker.com/reference/)para su administración, sin embargo, resulta mucho más cómodo utilizar una interfaz gráfica para el uso rutinario.

En [Visual Studio Code ](https://code.visualstudio.com/)podemos encontrar un [plugin ](https://code.visualstudio.com/docs/containers/overview)desarrollado por Microsoft que nos permite administrar las funciones principales de forma un poco más gráfica que el CLI.

![](<.gitbook/assets/Extensión\_ Docker - Visual Studio Code 25\_6\_2022 08\_42\_12.png>)

Una vez instalado nos permitirá:

* Ver el estado de los contenedores.
* Iniciar, detener, reiniciar y eliminar contenedores.
* Ingresar a la terminal de los contenedores.
* Visualizar el log de los contenedores.
* Gestionar imágenes.
* Crear y eliminar redes.
* Gestionar volúmenes.

Y muchas cosas más.

![](<.gitbook/assets/Docker extension for Visual Studio Code - Google Chrome 25\_6\_2022 08\_46\_39.png>)

![](<.gitbook/assets/Gestión de contenedores - Docker - Google Chrome 25\_6\_2022 08\_56\_35.png>)

Otra opción mucho más amigable (mi preferida) es [portainer](https://www.portainer.io/), una interfaz gráfica basada en web para Docker.

![](<.gitbook/assets/Docker extension for Visual Studio Code - Google Chrome 25\_6\_2022 09\_05\_58.png>)

Esta herramienta se instala en forma de contenedor, seria _**"un contenedor para gobernarlos a todos"**_, para ello crearemos un script docker-compose.yml:

```yaml
version: '3'

services:
  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    restart: unless-stopped
    ports:
      - "9000:9000"
    command: -H unix:///var/run/docker.sock
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer:/data
    networks:
      - default

volumes:
  portainer:

networks:
  default:
    external: true
    name: docker
```

Luego, ubicados en el directorio donde se encuentra el script ejecutamos:

```bash
docker-compose up -d
```

De esta manera se descargará la imagen del portainer y se creará un contenedor con la configuración especificada en el script (modificar según sus necesidades). En este caso se puede acceder a través del puerto 9000 del navegador.
