**Ejercicio 5: Uso de redes personalizadas**

Paso 1: Crea una red personalizada en Docker:

```bash
docker network create my_network
```

Paso 2: Modifica el archivo `docker-compose.yml` para que los servicios se unan a la red personalizada:

```yaml
version: '3'
services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    networks:
      - my_network
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: flaskapp
    networks:
      - my_network
networks:
  my_network:
    external: true
```

Paso 3: Levanta los servicios con Docker Compose:

```bash
docker-compose up -d
```
