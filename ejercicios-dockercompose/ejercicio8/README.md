**Ejercicio 8: Definir volúmenes específicos para persistencia de datos**

Paso 1: Define volúmenes específicos en el archivo `docker-compose.yml` para los servicios de MySQL y NGINX:

```yaml
version: '3'
services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - nginx_data:/usr/share/nginx/html
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: flaskapp
    volumes:
      - db_data:/var/lib/mysql
volumes:
  nginx_data:
  db_data:
```

Paso 2: Levanta los servicios con Docker Compose:

```bash
docker-compose up -d
```

