**Ejercicio 10: Uso de etiquetas de Docker Compose**

Paso 1: Define etiquetas para los servicios en el archivo `docker-compose.yml`:

```yaml
version: '3'
services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    labels:
      - "com.example.description=My web server"
      - "com.example.department=IT"
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: flaskapp
    labels:
      - "com.example.description=MySQL database"
      - "com.example.department=Data"
```

Paso 2: Visualiza las etiquetas de los contenedores con el siguiente comando:

```bash
docker-compose ps --filter "label=com.example.department=IT"
```