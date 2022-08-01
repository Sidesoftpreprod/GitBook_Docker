# Postgres

Script docker-compose.yml para crear los contenedores postgres y pgadmin4:

```yaml
version: "3.8"

services:
  postgres:
    image: postgres:10.21
    container_name: postgres
    restart: unless-stopped
    environment:
      POSTGRES_PASSWORD: postgres
      TZ: America/Guayaquil
    volumes:
      - postgres:/var/lib/postgresql/data
    ports:
      - 5432:5432
    command: postgres -c shared_preload_libraries=pg_stat_statements -c pg_stat_statements.track=all -c max_connections=200
    networks:
      - default
  
  pgadmin4:
    image: dpage/pgadmin4
    container_name: pgadmin4
    restart: unless-stopped
    environment:
      PGADMIN_DEFAULT_EMAIL: "EMAIL"
      PGADMIN_DEFAULT_PASSWORD: "PASSWORD"
    ports:
      - "5430:80"
    volumes:
      - pgadmin:/var/lib/pgadmin
    depends_on:
      - postgres
    networks:
      - default

volumes:
  postgres:
  pgadmin:

networks:
  default:
    external: true
    name: docker

```

Luego, ubicados en el directorio donde se encuentra el script ejecutamos:

```bash
docker-compose up -d
```

Con esto ya se puede acceder al pgadmin4 desde el navegador a través del puerto 5430 (modificar según sus necesidades).

![](<.gitbook/assets/pgAdmin 4 - Google Chrome 25\_6\_2022 11\_41\_12.png>)

Para conectarnos al contenedor de postgres desde el contenedor de pgadmin4 usamos su nombre de servicio definido en el script docker-compose.yml que en este caso es "postgres".

![](<.gitbook/assets/pgAdmin 4 - Google Chrome 25\_6\_2022 11\_41\_46.png>)

{% hint style="info" %}
Sí deseamos conectarnos desde el "Sistema Operativo Host" podemos hacerlo con localhost, 127.0.0.1 o la ip del host.
{% endhint %}
