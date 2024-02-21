#  EJERCICIOS DOCKERFILE


**Ejercicio 1: Crear un contenedor de Python básico**
Objetivo: Crear un contenedor Docker que ejecute un script de Python simple.

Dockerfile:
```Dockerfile
# Usa la imagen base de Python
FROM python:3.9

# Copia el script de Python al contenedor
COPY mi_script.py /mi_script.py

# Ejecuta el script al iniciar el contenedor
CMD ["python", "/mi_script.py"]
```

mi_script.py:
```python
print("¡Hola desde Docker!")
```

Para construir y ejecutar el contenedor:
```bash
# Construye la imagen
docker build -t mi_python_app .

# Ejecuta el contenedor
docker run mi_python_app
```

**Ejercicio 2: Agregar dependencias a una aplicación Flask**
Objetivo: Extender el Dockerfile anterior para incluir dependencias de una aplicación Flask.

Dockerfile:
```Dockerfile
# Usa la imagen base de Python
FROM python:3.9

# Copia los archivos necesarios al contenedor
COPY requirements.txt /app/requirements.txt
COPY mi_app_flask.py /app/mi_app_flask.py

# Establece el directorio de trabajo
WORKDIR /app

# Instala las dependencias
RUN pip install -r requirements.txt

# Ejecuta la aplicación Flask
CMD ["python", "mi_app_flask.py"]
```

requirements.txt:
```
Flask==2.1.0
```

mi_app_flask.py:
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return '¡Hola desde Flask en Docker!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

Para construir y ejecutar el contenedor:
```bash
# Construye la imagen
docker build -t mi_flask_app .

# Ejecuta el contenedor
docker run -p 5000:5000 mi_flask_app
```

**Ejercicio 3: Agregar un usuario no root al contenedor**
Objetivo: Mejorar la seguridad del contenedor creando un usuario no root para ejecutar la aplicación.

Dockerfile:
```Dockerfile
# Usa la imagen base de Python
FROM python:3.9

# Copia los archivos necesarios al contenedor
COPY requirements.txt /app/requirements.txt
COPY mi_app_flask.py /app/mi_app_flask.py

# Establece el directorio de trabajo
WORKDIR /app

# Agrega un usuario no root
RUN useradd -m miusuario

# Cambia al usuario no root
USER miusuario

# Instala las dependencias
RUN pip install -r requirements.txt

# Ejecuta la aplicación Flask
CMD ["python", "mi_app_flask.py"]
```

**Ejercicio 4: Utilizar una imagen base más ligera**
Objetivo: Optimizar el tamaño del contenedor utilizando una imagen base más ligera, como Alpine.

Dockerfile:
```Dockerfile
# Usa la imagen base de Python Alpine
FROM python:3.9-alpine

# Copia los archivos necesarios al contenedor
COPY requirements.txt /app/requirements.txt
COPY mi_app_flask.py /app/mi_app_flask.py

# Establece el directorio de trabajo
WORKDIR /app

# Agrega un usuario no root
RUN adduser -D miusuario

# Cambia al usuario no root
USER miusuario

# Instala las dependencias
RUN pip install -r requirements.txt

# Ejecuta la aplicación Flask
CMD ["python", "mi_app_flask.py"]
```

**Ejercicio 5: Optimizar la construcción del contenedor**
Objetivo: Minimizar las capas de la imagen Docker para mejorar la eficiencia y el rendimiento.

Dockerfile:
```Dockerfile
# Etapa de construcción
FROM python:3.9-alpine as builder

# Copia los archivos necesarios al contenedor
COPY requirements.txt /app/requirements.txt

# Instala las dependencias
RUN pip install --prefix=/install -r /app/requirements.txt

# Etapa de producción
FROM python:3.9-alpine

# Copia los archivos necesarios al contenedor
COPY --from=builder /install /usr/local

COPY mi_app_flask.py /app/mi_app_flask.py

# Establece el directorio de trabajo
WORKDIR /app

# Agrega un usuario no root
RUN adduser -D miusuario

# Cambia al usuario no root
USER miusuario

# Ejecuta la aplicación Flask
CMD ["python", "mi_app_flask.py"]
```

Estos ejercicios proporcionan una base sólida para comenzar a trabajar con Docker y Dockerfile. ¡Espero que te sean útiles para aprender y practicar!


#  EJERCICIOS MULTI STAGE

¡Por supuesto! Aquí tienes dos ejercicios adicionales utilizando multi-stage builds:

**Ejercicio 6: Compilación de una aplicación Go con Docker**
Objetivo: Compilar una aplicación Go en un contenedor Docker utilizando multi-stage builds.

Dockerfile:
```Dockerfile
# Etapa de construcción
FROM golang:1.17-alpine as builder

# Establece el directorio de trabajo
WORKDIR /app

# Copia el código fuente
COPY . .

# Compila la aplicación
RUN go build -o mi_app .

# Etapa de producción
FROM alpine:latest

# Copia el binario compilado de la etapa de construcción
COPY --from=builder /app/mi_app /usr/local/bin/mi_app

# Ejecuta la aplicación al iniciar el contenedor
CMD ["mi_app"]
```

Para construir y ejecutar el contenedor:
```bash
# Construye la imagen
docker build -t mi_app_go .

# Ejecuta el contenedor
docker run mi_app_go
```

**Ejercicio 7: Construcción de una aplicación Angular con Docker**
Objetivo: Construir una aplicación Angular en un contenedor Docker utilizando multi-stage builds.

Dockerfile:
```Dockerfile
# Etapa de compilación
FROM node:14 as builder

# Establece el directorio de trabajo
WORKDIR /app

# Copia el archivo de configuración de paquetes
COPY package.json package-lock.json ./

# Instala las dependencias
RUN npm install

# Copia el código fuente
COPY . .

# Compila la aplicación Angular
RUN npm run build --prod

# Etapa de producción
FROM nginx:latest

# Copia los archivos estáticos compilados de la etapa de compilación
COPY --from=builder /app/dist/my-angular-app /usr/share/nginx/html

# Configura el servidor Nginx
EXPOSE 80

# Inicia Nginx al iniciar el contenedor
CMD ["nginx", "-g", "daemon off;"]
```

Para construir y ejecutar el contenedor:
```bash
# Construye la imagen
docker build -t mi_angular_app .

# Ejecuta el contenedor
docker run -p 8080:80 mi_angular_app
```

Estos ejercicios demuestran cómo utilizar multi-stage builds en Docker para optimizar la construcción de imágenes y reducir el tamaño final del contenedor.