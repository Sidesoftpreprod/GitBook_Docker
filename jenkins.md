# Jenkins

Script docker-compose.yml para crear contenedores jenkins:

```yaml
version: "3.8"
services:
  jenkins:
    image: jenkinsci/blueocean
    container_name: jenkins
    user: root
    restart: unless-stopped
    environment:
      TZ: America/Guayaquil
    ports:
      - "8100:8080"
      - "8443:8443"
      - "50000:50000"
    volumes:
      - jenkins:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
    networks:
      - default

volumes:
  jenkins:

networks:
  default:
    external: true
    name: docker

```

Luego, ubicados en el directorio donde se encuentra el script ejecutamos:

```bash
docker-compose up -d
```
