![](https://github.com/GEEKSHUBS-DEVOPS2020/trabajando-con-contenedores/blob/master/logo.png?raw=true)

# Trabajando con contenedores


```
Fecha: 31 de enero y 1 de febrero de 2020
Profesor: David Pestana
Horas: 10
```

En esta unidad vamos a entender los conceptos fundamentales de la contenerización, aprenderemos a manipular y controlar lo que sucede dentro de nuestros contenedores, como interactuan los contenedores tanto entre si como con el sistema anfitrión.  Aprenderemos a construir imágenes a medida que cubran las necesidades o requisitos de cada proyecto.

Trabajaremos en concreto con Docker ya que es el sistema de contenedores mas extendido y popular.

El 90% de esta unidad sera totalmente práctica, por lo que es importante que se cumplan los siguientes requisitos al comenzar la clase.

## setup
* docker y docker-compose instalado y operativo en tu equipo.

existen muchos mecanismos para instalar tanto docker como docker-compose en los diferentes sistemas operativos, y por tanto los problemas que nos podemos encontrar durante la clase al respecto pueden ser variados y aunque podemos tratar de resolverlos seria conveniente que ya los traigáis, instalados y funcionando. No obstante lo mas recomendable si no tienes un sistema anfitrión Linux, es que uses algún sistema linux virtualizado.


## contenidos
-   Conceptos
	- Imagen y Contenedor, 
	- Orquestación.
-   Prácticas básicas
    -   Ejecutando un contenedor
    -   Interactuando con el anfitrión
	    - Pasar variables de entorno a un contenedor
	    - Montando directorios en el anfitrión
	    - Exponiendo puertos al anfitrión
    -	Listando contenedores
    -	Deteniendo, iniciando y reiniciando contenedores
    -	Eliminar contenedores
    -	Ejecutando un comando dentro de un contenedor en ejecución.
    -	Accediendo a un contenedor en ejecución.
    - Personalizando nuestra imagen
	    - El archivo Dockerfile
  - Montar entornos de desarrollo contenerizados.
	   - Ventajas y Desventajas
	   - Orquestación con docker-compose
	   - Tecnicas de Scripting
	   - Repositorio
	   - Facilitar el Build & Deploy 
   - Entornos de Producción
	   - vistos en la unidad anterior ( Desplegando contenedores )
		  

> **dia 1**

# Conceptos	

> que es y que no es una imagen
> que es y que no es un contenedor
> contenedores **stateless** y **stateful**
> que es la orquestación de contenedores

# Prácticas

## ejecutando un contenedor
todos los contenedores necesitan tanto un id como un nombre, si nosotros no nombramos un contenedor Docker le dará un nombre fácil de recordar basado en unos diccionarios generadores, sin embargo el id siempre sera auto-generado.

    docker run hello-world
	docker run --name hello-world hello-world

## listando contendores 

    docker ps
    docker ps -a

> ¿comprendemos lo que sucede?  
> ¿puedes explicar que ves en esta tabla?

## ejecutar un contenedor Auto-Remover

    docker run --rm hello-world

> vuelve a listar los contenedores.
> ¿ que hace el argumento --rm ? 
> 
## ejecutar diferentes comandos en un contenedor
Por defecto, Docker ejecuta el comando especificado al construir la imagen cuando ejecutas un contenedor, sin embargo podemos especificar cualquier otro comando que este disponible en el contenedor.

    docker run hello-world ls -lah

> ¿ que ocurre ?
>  ¿ por que ocurre ?

    docker run busybox ls -lah

>  ¿ y ahora ?

## ejecutar contenedores interactivos
en muchas ocasiones, un proceso desencadenado a traves de un comando en un sistema requiere una interaccion humana, a través del **stdin**, cuando ejecutamos un comando en un contenedor, este interactua internamente dentro de dicho contendor, y por tanto no vamos a tener acceso al **stdin**

    docker run --rm -it busybox

al usar el argumento -it conectamos el **stdin** del comando ejecutado en el contenedor con la entrada del anfitrión

    docker run --rm -it busybox vi

## montando directorios y volúmenes, variables de entorno
Una de las interacciones más útiles es montar directorios del contenedor en el anfitrión para diferentes casos de uso.
-   Almacenamiento compartido entre contenedores corriendo sobre el mismo anfitrión.  ( sesiones, archivos de configuración)
-   Ver y editar archivos usando tu entorno anfitrión y herramientas y usando los archivos en el contenedor (development)
-   Persistencia a nivel de anfitrión más allá del tiempo de vida de un contenedor, ( bases de datos )

```
docker run --name custom-mysql -v ./custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:5.7
```
```
docker run --name persisted-mysql -v ./data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:5.7
```

> ¿ identificas los casos de uso sobre estos dos contenedores de mysql ?
> habitualmente, la manera de parametrizar el comportamiento del comando ejecutado en un contendor es a través de las variables de entorno, que se pasan al contenedor con el argumento **-e**

## exponiendo puertos
Al construir una imagen podemos exponer un puerto determinado, por ejemplo en el caso de mysql expondrá el puerto estándar 3306, pero este puerto solo sera visible por otros contenedores de su red, nunca por el anfitrión y por tanto tampoco sera visible por las capas superiores, como la red del anfitrión o Internet.

mediante el argumento **-p** podemos mapear los puertos expuestos por el contenedor al anfitrión 

    docker run --name nginx --rm -d -p 9000:80 nginx

> listemos los contenedores con docker ps
> vemos que nginx a diferencia de los contenedores anteriores no se detiene y continua vivo esperando conexiones a través del puerto 80 mapeado al puerto 9000 del sistema anfitrión.
> esto se debe a que la salida **stdout** del proceso de nginx es infinita y un contenedor docker termina su ejecución cuando la salida estándar del proceso finaliza su emisión.


## listando contenedores avanzado
La salida de `docker ps` puede ser demasiado verbosa y frecuentemente muestra muchos campos que no son interesantes. 

### formateado
Puedes usar formato Go-template para mostrar solo los campos que te interesen. Aquí está mostrando solo el nombre y el comando:

       docker ps --no-trunc --format '{{.Names}}\t{{.Command}}'


### filtrado
El comando `docker ps` soporta muchos filtros. Son bastante sencillos de usar. La sintaxis es -f "<filtro>=<valor>". Los filtros soportados son id, label, name, exited, status, ancestor, before, since, isolation, network, y health.

    `docker ps -a -f "name=busybox" --format 'table {{.ID}}\t{{.Status}}\t{{.Names}}'`

### bandera **-q**
Si por ejemplo solo quieres el ID, (esto es útil para componer scripts o sentencias conectadas a los que quieres pasar una colección de ids de contenedor ) 

    docker ps -aq

## deteniendo, iniciando y reiniciando contenedores
tras arrancar un contenedor si no lo destruimos, o ejecutamos con la opción auto-remove, este contenedor permanecerá disponible para que podamos operar con el.
No es lo mismo iniciar, o reiniciar un contenedor que ejecutar un contenedor, ya que cuando lo inicias, o reinicias usa las mismas variables de entorno, volúmenes, puertos que su ejecución original.

### deteniendo

    docker stop nginx
### iniciando

    docker start nginx
### reiniciando

    docker restart nginx

## Destruyendo contenedores
solo se puede destruir un contenedor detenido mediante el comando rm seguido tanto de su nombre como de su id

    docker rm custom-mysql
si quieres remover un contenedor en ejecución puedes usar el forzado con el argumento **-f**

### destruir todos los contenedores

    docker rm -f $(docker ps -aq)


## Ejecutar comandos dentro de contenedores en ejecución
a diferencia de **run** la función **exec** no genera un nuevo contenedor si no que accede a uno que ya esta en ejecución para ejecutar el comando que se le indique.

    docker exec nginx cat /etc/nginx/nginx.conf | grep worker_processes

### comandos interactivos
al igual que con la funcion **run** podemos lanzar con **exec** cualquier comando añadiendo **--it** para obtener interactividad a través de la entrada **stdin**.
esto nos permite ejecutar con interactividad el comando mas potente y que mas nos va a permitir interactuar con nuestro contenedor el **Shell** siempre y cuando este disponible en el contenedor. ( habitualmente bash o /bin/bash )

# Shell interactivo

    docker exec -it nginx bash


# Personalizando nuestra imagen
## el archivo Dockerfile

Es un archivo de configuración que se utiliza para crear imágenes. En dicho archivo indicamos qué es lo que queremos que tenga la imagen, y los distintos comandos para instalar las herramientas. Esto sería un ejemplo de **Dockerfile** para tener una imagen de Ubuntu con Git instalado:

    FROM ubuntu:latest
    RUN apt-get update
    RUN apt-get -qqy install git

> ejercicio: crea un archivo llamado Dockerfile y edita su contenido con el código anterior

## creando la imagen

### sin nombre
docker le pondra a nuestra nueva imagen un nombre auto-generado

    docker build -t .

### con nombre
docker creara una imagen con el nombre custom-ubuntu-git a partir del Dockerfile

    docker build -t custom-ubuntu-git .
    

> **dia 2**

  # Entornos de desarrollo en contenedores
  a menudo nuestro equipo de desarrollo se puede enfrentar a la necesidad de trabajar con proyectos basados en diferentes stacks, con diversos contextos que habitualmente pueden ser incluso incompatibles entre si.  Por tanto una buena filosofía DevOps es la de facilitar el setup de un proyecto a los developers ya que en esta tarea se puede invertir mucho tiempo cada vez que un developer cambia de proyecto o se actualiza un repo después de cierto tiempo.
### ventajas
  *	el tiempo y esfuerzo de setup del entorno de desarrollo se reduce radicalmente.
  *	se eliminan problemas de dependencias de versiones entre developers y entornos.
  *	el entorno de desarrollo y el de producción son idénticos.
  *	el contexto de desarrollo es el mismo para todos los developers.
 

>  ¿ se nos ocurren mas ventajas ?

### desventajas
* el uso de herramientas cliente o facilidades de los propios frameworks de desarrollo se vuelve mas compleja por tener que ser aplicada dentro de los contenedores y no estar disponibles en el anfitrión.

## Orquestación con docker-compose
Docker Compose es una herramienta de orquestación  que permite simplificar el uso de Docker, generando scripts que facilitan el diseño y la construcción de servicios.

## Tecnicas de Scripting
Algunos comandos o funciones repetitivas relativas a un proyecto en concreto, pueden incluirse en el repositorio de desarrollo de manera que faciliten a los desarrolladores y a todas las partes involucradas la ejecución de esas tareas.
Tareas que se repiten tras clonar el repositorio como, arrancar todos los servicios, parar todos los servicios, acceder a un contenedor para ejecutar un comado concreto, instalar librerias... etc.
Si adjuntamos una colección de scripts, no solo lograremos que se trabaje con cierta homogeneidad en el equipo, si no que ademas aceleraremos todos sus procesos.

## Repositorio full-equip
Este concepto se basa en la idea de incluir en el repositorio del proyecto todas las herramientas y scripts necesarios, así como un correcto Readme que explique con detalle el uso de cada una de estas herramientas.  El valor que aporta esto a un equipo de desarrollo justifica con creces cada hora invertida en esta labor.

# Práctica 1
el equipo de desarrollo va a comenzar el desarrollo de una solución basada en node 10 para construir algunos micro-servicios, con bases de datos mongo latest version, que ademas compartirán un servicio redis para resolver cierta especificación del proyecto y un servicio Minio para depositar archivos. Debemos dotar un repositorio con todas las facilidades y los entornos de desarrollo bajo las versiones requeridas que ayude a agilizar al máximo el esfuerzo de cada uno de los miembros del equipo de desarrollo.

> repositorio con la practica terminada:
https://github.com/GEEKSHUBS-DEVOPS2020/trabajando-con-contenedores-practica-uno

# Práctica 2
se nos pide dotar y desplegar un prestaShop para que una agencia de marketing que hemos sub-contratado pueda integrar un diseño maquetado por ellos, mientras nuestros developers desarrollan ciertos modulos requeridos.
Dotaremos de un repositorio en el que todos los stakeHolders puedan trabajar ordenadamente y a traves del cual mas adelante podremos conectar los pipelines necesarios.

> repositorio con la práctica terminada:
https://github.com/GEEKSHUBS-DEVOPS2020/trabajando-con-contenedores-practica-dos


