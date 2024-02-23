**Ejercicio 15: Docker Compose con una aplicación PHP escalada y NGINX como balanceador de carga**

Este ejercicio implica crear un entorno Docker Compose que incluya una aplicación PHP escalada en varios contenedores y un servicio NGINX como balanceador de carga.

Paso 1: Crea una aplicación PHP simple.

Paso 2: Define el archivo `Dockerfile` para la aplicación PHP:

```Dockerfile
FROM php:7.4-apache

COPY src/ /var/www/html/
```

Paso 3: Define el archivo `docker-compose.yml`:

```yaml
version: '3'
services:
  php:
    build: .
    ports:
      - "8080"
  nginx:
    image: nginx:latest
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    ports:
      - "80:80"
```

Paso 4: Crea un archivo `nginx.conf` en el directorio del proyecto:

```nginx
events {}

http {
    upstream php_servers {
        server php1:80;
        server php2:80;
        server php3:80;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://php_servers;
        }
    }
}
```

Paso 5: Ejecuta `docker-compose up -d --scale php=3` para iniciar los servicios y escalar la aplicación PHP a 3 réplicas.

Paso 6: Accede a la aplicación PHP a través del balanceador de carga NGINX en `http://localhost`.