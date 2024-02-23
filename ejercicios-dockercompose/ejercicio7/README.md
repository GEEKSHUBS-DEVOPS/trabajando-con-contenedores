**Ejercicio 7: Definir una red específica para los servicios**

Paso 1: Define una red personalizada en el archivo `docker-compose.yml`:

```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - db
    networks:
      - my_network
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: flaskapp
    ports:
      - "3306:3306"
    networks:
      - my_network
networks:
  my_network:
```

Paso 2: Levanta los servicios con Docker Compose:

```bash
docker-compose up -d
```