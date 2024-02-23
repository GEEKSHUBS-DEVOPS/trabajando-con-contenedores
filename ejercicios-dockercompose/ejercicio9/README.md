**Ejercicio 9: Uso de Docker Compose para despliegue en producción con nginx como balanceador de carga**

Paso 1: Crea un archivo `nginx.conf` en el directorio del proyecto:

```bash
touch nginx.conf
```

Dentro de `nginx.conf`:

```nginx
events {}

http {
    upstream flask_app {
        server web1:5000;
        server web2:5000;
        server web3:5000;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://flask_app;
        }
    }
}
```

Paso 2: Modifica el archivo `docker-compose.yml` para usar NGINX como balanceador de carga y para incluir el archivo de configuración personalizado:

```yaml
version: '3'
services:
  web1:
    build: .
    ports:
      - "5001:5000"
  web2:
    build: .
    ports:
      - "5002:5000"
  web3:
    build: .
    ports:
      - "5003:5000"
  nginx:
    image: nginx:latest
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    ports:
      - "80:80"
```

Paso 3: Levanta los servicios con Docker Compose:

```bash
docker-compose up -d
```