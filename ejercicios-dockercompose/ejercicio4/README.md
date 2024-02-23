**Ejercicio 4: Configurar volúmenes para persistencia de datos**

Paso 1: Modifica el archivo `docker-compose.yml` para agregar volúmenes a los servicios de MySQL y NGINX:

```yaml
version: '3'
services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./nginx:/etc/nginx/conf.d
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: flaskapp
    volumes:
      - db_data:/var/lib/mysql
volumes:
  db_data:
```

Paso 2: Crea un directorio `nginx` en tu directorio de proyecto y agrega un archivo de configuración `default.conf`:

```bash
mkdir nginx
```

Dentro de `nginx/default.conf`:

```nginx
server {
    listen 80;
    server_name localhost;

    location / {
        root /usr/share/nginx/html;
        index index.html;
    }
}
```

Paso 3: Levanta los servicios con Docker Compose:

```bash
docker-compose up -d
```

