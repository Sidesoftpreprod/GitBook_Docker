---
description: 'Script docker-compose.yml para crear contenedores tomcat:'
---

# Tomcat

```yaml
version: "3.8"

services:
  tomcat:
    environment:
      TZ: America/Guayaquil
    image: tomcat:8.5-jdk8-openjdk
    container_name: tomcat
    restart: unless-stopped
    volumes:
      - /opt/webapps:/usr/local/tomcat/webapps
    working_dir: /opt/webapps
    ports:
      - 8080:8080
    networks:
      - default

networks:
  default:
    external: true
    name: docker
```

Luego, ubicados en el directorio donde se encuentra el script ejecutamos:

```bash
docker-compose up -d
```

Con esto ya se puede desplegar cualquier aplicación Java que coloquemos en el directorio /opt/webapps
