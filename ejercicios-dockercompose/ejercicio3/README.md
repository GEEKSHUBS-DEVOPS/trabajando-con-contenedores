**Ejercicio 3: Escalar servicios con Docker Compose**

Paso 1: Modifica el archivo `docker-compose.yml` del ejercicio anterior para escalar el servicio web a 3 réplicas:

```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "5000-5002:5000"
    depends_on:
      - db
    scale: 3
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: flaskapp
```

Paso 2: Escala el servicio web:

```bash
docker-compose up -d --scale web=3
```

