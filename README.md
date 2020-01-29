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

