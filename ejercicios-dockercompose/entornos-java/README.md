**Ejercicio 11: Docker Compose con una aplicación Spring Boot y MySQL**

Este ejercicio implica crear un entorno Docker Compose que incluya una aplicación Spring Boot y un servidor MySQL.

Paso 1: Crea una aplicación Spring Boot simple.

Paso 2: Define el archivo `Dockerfile` para la aplicación Spring Boot:

```Dockerfile
FROM openjdk:11-jre-slim

WORKDIR /app

COPY target/myapp.jar /app

CMD ["java", "-jar", "myapp.jar"]
```

Paso 3: Define el archivo `docker-compose.yml`:

```yaml
version: '3'
services:
  myapp:
    build: .
    ports:
      - "8080:8080"
    depends_on:
      - db
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: mydatabase
    ports:
      - "3306:3306"
```

Paso 4: Ejecuta `docker-compose up -d` para iniciar los servicios.

Paso 5: Accede a la aplicación Spring Boot en `http://localhost:8080`.

**Ejercicio 12: Docker Compose con múltiples servicios Java y MongoDB**

Este ejercicio implica crear un entorno Docker Compose con múltiples servicios Java y una base de datos MongoDB.

Paso 1: Crea varias aplicaciones Java simples.

Paso 2: Define los archivos `Dockerfile` para cada aplicación Java.

Paso 3: Define el archivo `docker-compose.yml`:

```yaml
version: '3'
services:
  service1:
    build:
      context: ./service1
    ports:
      - "8081:8080"
    depends_on:
      - mongodb
  service2:
    build:
      context: ./service2
    ports:
      - "8082:8080"
    depends_on:
      - mongodb
  mongodb:
    image: mongo:latest
    ports:
      - "27017:27017"
```

Paso 4: Ejecuta `docker-compose up -d` para iniciar los servicios.

Paso 5: Accede a las aplicaciones Java en `http://localhost:8081` y `http://localhost:8082`.
