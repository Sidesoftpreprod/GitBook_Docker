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

Cuando el contenedor termine de construirse tendremos que buscar en el log la clave de activación:

```bash
docker logs -f jenkins
```

![](<.gitbook/assets/Jenkins - Docker - Opera 1\_8\_2022 09\_23\_00.png>)

Accedemos a través del navegados al [jenkins](http://localhost:8100), la primera vez tendremos que configurar y haremos uso de la clave de activación:

![](<.gitbook/assets/Opera Instantánea\_2022-08-01\_092841\_localhost.png>)

Se deben seguir las instrucciones del asistente de configuración

![](<.gitbook/assets/Opera Instantánea\_2022-08-01\_092854\_localhost.png>)

![](<.gitbook/assets/Opera Instantánea\_2022-08-01\_092905\_localhost.png>)

![](<.gitbook/assets/Opera Instantánea\_2022-08-01\_093308\_localhost.png>)

Una vez finalizado ya podremos empezar a crear nuestros Jobs

![](<.gitbook/assets/Opera Instantánea\_2022-08-01\_093534\_localhost.png>)

![](<.gitbook/assets/Opera Instantánea\_2022-08-01\_093522\_localhost.png>)
