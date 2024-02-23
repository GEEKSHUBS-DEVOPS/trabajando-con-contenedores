**Ejercicio 1: Crear y ejecutar un contenedor de NGINX:**

Paso 1: Crea un directorio para tu proyecto.

```bash
mkdir proyecto_docker
cd proyecto_docker
```

Paso 2: Crea un archivo `docker-compose.yml` con el siguiente contenido:

```yaml
version: '3'
services:
  nginx:
    image: nginx:latest
    ports:
      - "80:80"
```

Paso 3: Ejecuta el contenedor usando Docker Compose:

```bash
docker-compose up -d
```

Paso 4: Accede a http://localhost en tu navegador para ver la página de bienvenida de NGINX.

