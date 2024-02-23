**Ejercicio 6: Uso de variables de entorno**

Paso 1: Modifica el archivo `docker-compose.yml` para usar variables de entorno en la configuración del servicio Flask:

```yaml
version: '3'
services:
  web:
    build:
      context: .
      args:
        - FLASK_ENV=development
    ports:
      - "5000:5000"
    environment:
      - MYSQL_HOST=db
      - MYSQL_USER=root
      - MYSQL_PASSWORD=example
      - MYSQL_DB=flaskapp
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: flaskapp
    ports:
      - "3306:3306"
```

Paso 2: Modifica tu archivo `app.py` para usar estas variables de entorno:

```python
from flask import Flask
import mysql.connector
import os

app = Flask(__name__)

MYSQL_HOST = os.environ.get('MYSQL_HOST', 'localhost')
MYSQL_USER = os.environ.get('MYSQL_USER', 'root')
MYSQL_PASSWORD = os.environ.get('MYSQL_PASSWORD', 'example')
MYSQL_DB = os.environ.get('MYSQL_DB', 'flaskapp')

@app.route('/')
def index():
    db_connection = mysql.connector.connect(
        host=MYSQL_HOST,
        user=MYSQL_USER,
        password=MYSQL_PASSWORD,
        database=MYSQL_DB
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

Paso 3: Levanta los servicios con Docker Compose:

```bash
docker-compose up -d
```

