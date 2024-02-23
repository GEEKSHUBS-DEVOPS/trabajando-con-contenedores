**Ejercicio 13: Docker Compose con una aplicación Node.js y MongoDB**

Este ejercicio implica crear un entorno Docker Compose que incluya una aplicación Node.js y un servidor MongoDB.

Paso 1: Crea una aplicación Node.js simple.

Paso 2: Define el archivo `Dockerfile` para la aplicación Node.js:

```Dockerfile
FROM node:14

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm install

COPY . .

CMD ["npm", "start"]
```

Paso 3: Define el archivo `docker-compose.yml`:

```yaml
version: '3'
services:
  nodeapp:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - mongodb
  mongodb:
    image: mongo:latest
    ports:
      - "27017:27017"
```

Paso 4: Ejecuta `docker-compose up -d` para iniciar los servicios.

Paso 5: Accede a la aplicación Node.js en `http://localhost:3000`.

**Ejercicio 14: Docker Compose con una aplicación Node.js y un servidor de Redis**

Este ejercicio implica crear un entorno Docker Compose que incluya una aplicación Node.js y un servidor Redis.

Paso 1: Crea una aplicación Node.js simple que use Redis.

Paso 2: Define el archivo `Dockerfile` para la aplicación Node.js:

```Dockerfile
FROM node:14

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm install

COPY . .

CMD ["npm", "start"]
```

Paso 3: Define el archivo `docker-compose.yml`:

```yaml
version: '3'
services:
  nodeapp:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - redis
  redis:
    image: redis:latest
    ports:
      - "6379:6379"
```

Paso 4: Ejecuta `docker-compose up -d` para iniciar los servicios.

Paso 5: Accede a la aplicación Node.js en `http://localhost:3000`.
