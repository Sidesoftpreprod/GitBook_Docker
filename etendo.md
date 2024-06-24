---
description: >-
  El contenedor de etendo requiere unos archivos o configuraciones adicionales
  similares a los utilizados para Openbravo.
---

# Etendo

Script init.sh para configurar la base de datos requiere crearlo dentro del directorio postgres:

```bash
psql -U postgres -c "CREATE USER tad WITH SUPERUSER PASSWORD 'tad';"
sed -i -e"s/^datestyle = 'iso, mdy'.*$/datestyle = 'iso, dmy'/" /var/lib/postgresql/data/postgresql.conf
```

Script Dockerfile para crear contenedores etendo requiere crearlo dentro del directorio tomcat:

```docker
FROM tomcat:9-jdk11-openjdk

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update
RUN apt-get install -y ant nano git zip

RUN mkdir /usr/local/tomcat/conf/Catalina
RUN chmod 777 -R /usr/local/tomcat/webapps

RUN cd /tmp \
    && git clone --depth 1 -b 22.4.4 https://github.com/etendosoftware/etendo_core.git \
    && mv etendo_core /opt/etendo

RUN chmod 777 -R /opt/etendo

RUN adduser --disabled-password --gecos '' etendo

USER etendo

RUN cd /opt/etendo \
    && cp gradle.properties.template gradle.properties \
    && echo 'bbdd.host=postgres-etendo' >> gradle.properties

RUN cd /opt/etendo && ./gradlew setup

    # ./gradlew update.database smartbuild
    #./gradlew install smartbuild
```

Script docker-compose.yml para crear contenedores etendo:

```yaml
version: '3.8'

services:
  etendo:
    build:
      context: .
      dockerfile: tomcat/Dockerfile
    #image: rarc88/etendo:22.4.4
    image: etendo:22.4.4-lts
    container_name: etendo
    restart: unless-stopped
    environment:
      TZ: America/Guayaquil
      JPDA_ADDRESS: 8000
      JPDA_TRANSPORT: dt_socket
    volumes:
      - webapps-etendo:/usr/local/tomcat/webapps
    working_dir: /opt/etendo
    ports:
      - 18000:8000
      - 18080:8080
    networks:
      - default
    depends_on:
      - postgres
    # command to execute
    command:
      catalina.sh jpda run
    user: etendo

  postgres:
    image: postgres:13
    container_name: postgres-etendo
    restart: unless-stopped
    environment:
      TZ: America/Guayaquil
      POSTGRES_PASSWORD: syspass
    volumes:
      - postgres-etendo:/var/lib/postgresql/data
      - ./postgres/init.sh:/docker-entrypoint-initdb.d/init.sh
    ports:
      - 6432:5432
    networks:
      - default

volumes:
  webapps-etendo:
  postgres-etendo:

networks:
  default:
    name: etendo
```

Luego, ubicados en el directorio donde se encuentra el script ejecutamos:

```bash
docker-compose up -d
```

Los siguientes pasos solo son necesarios realizarlos la primera vez en caso de no tener un ambiente previo de **etendo**:

```bash
docker exec -it etendo ./gradlew install smartbuild
```
