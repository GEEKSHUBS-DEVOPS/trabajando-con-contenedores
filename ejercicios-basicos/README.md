Ejercicios para la comprension de los conceptos básicos de Docker

1. **Creación de un contenedor básico**: 
   - Ejercicio: Crea un contenedor Docker a partir de la imagen `ubuntu` y ejecuta el comando `echo "Hola Mundo"` dentro del contenedor.

2. **Control de proceso en un contenedor**: 
   - Ejercicio: Ejecuta un contenedor basado en la imagen `nginx` y verifica su estado utilizando los comandos `docker ps` y `docker stats`.

3. **Exposición de puertos**:
   - Ejercicio: Crea un contenedor basado en la imagen `httpd` (Apache HTTP Server) y expón el puerto 80 del contenedor al puerto 8080 del host. Accede al servidor Apache desde un navegador web.

4. **Creación de una red Docker**:
   - Ejercicio: Crea una red personalizada en Docker llamada `mi-red` utilizando el comando `docker network create`. Luego, crea dos contenedores en esta red y verifica su conectividad entre sí.

5. **Montaje de volúmenes**:
   - Ejercicio: Crea un contenedor basado en la imagen `mysql` y monta un volumen para persistir los datos de la base de datos en el host. Verifica que los datos persistan incluso después de detener y eliminar el contenedor.

6. **Uso de variables de entorno**:
   - Ejercicio: Crea un contenedor basado en la imagen `nginx` y utiliza variables de entorno para configurar el puerto en el que el servidor Nginx escuchará. Luego, accede al servidor Nginx utilizando el puerto especificado por la variable de entorno.

7. **Conexión entre contenedores sin Docker Compose**:
   - Ejercicio: Crea dos contenedores, uno basado en la imagen `mysql` y otro en la imagen `php`, y configura el contenedor de PHP para que se conecte al servidor MySQL utilizando la red predeterminada de Docker.

8. **Compartir datos entre contenedores**:
   - Ejercicio: Crea un contenedor basado en la imagen `nginx` y otro en la imagen `php`. Monta un volumen compartido entre ambos contenedores y modifica un archivo PHP desde el contenedor de Nginx, observa los cambios reflejados en el contenedor de PHP.

9. **Gestión de imágenes Docker**:
   - Ejercicio: Utiliza los comandos `docker pull`, `docker build` y `docker push` para descargar, construir y subir una imagen personalizada de Docker a Docker Hub.

10. **Documentación y búsqueda de imágenes**:
   - Ejercicio: Investiga en Docker Hub una imagen de tu interés. Lee su documentación, observa sus etiquetas y versiones disponibles. Luego, descarga y ejecuta un contenedor basado en esta imagen.

Estos ejercicios deberían proporcionar una buena comprensión de los conceptos básicos de Docker, incluyendo la creación de contenedores, control de procesos, redes y exposición de puertos, manejo de volúmenes y variables de entorno, así como la interacción entre contenedores.