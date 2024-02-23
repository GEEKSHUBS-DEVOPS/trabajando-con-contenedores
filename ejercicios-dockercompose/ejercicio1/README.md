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

**Ejercicio 2: Crear una aplicación web con Flask y MySQL:**

Paso 1: Crea un directorio para tu proyecto.

```bash
mkdir proyecto_flask_mysql
cd proyecto_flask_mysql
```

Paso 2: Crea un archivo `Dockerfile` para la aplicación Flask:

```Dockerfile
FROM python:3.8-slim

WORKDIR /app

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

Paso 3: Crea un archivo `docker-compose.yml` con el siguiente contenido:

```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - db
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: flaskapp
    ports:
      - "3306:3306"
```

Paso 4: Crea un archivo `requirements.txt` con las dependencias de Flask.

```txt
Flask==1.1.2
mysql-connector-python==8.0.23
```

Paso 5: Crea tu aplicación Flask en un archivo `app.py`.

```python
from flask import Flask
import mysql.connector

app = Flask(__name__)

@app.route('/')
def index():
    db_connection = mysql.connector.connect(
        host="db",
        user="root",
        password="example",
        database="flaskapp"
    )
    cursor = db_connection.cursor()
    cursor.execute("CREATE TABLE IF NOT EXISTS visits (id INT AUTO_INCREMENT PRIMARY KEY, timestamp TIMESTAMP)")
    cursor.execute("INSERT INTO visits (timestamp) VALUES (NOW())")
    cursor.execute("SELECT COUNT(*) FROM visits")
    count = cursor.fetchone()[0]
    db_connection.commit()
    db_connection.close()
    return f'Número de visitas: {count}'

if __name__ == '__main__':
    app.run(host='0.0.0.0')
```

Paso 6: Ejecuta la aplicación con Docker Compose:

```bash
docker-compose up -d
```

Paso 7: Accede a http://localhost:5000 en tu navegador para ver la aplicación Flask.