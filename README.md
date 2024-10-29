## PRACTICA 4






### CREAR UNA RED DOCKER 



Una vez inciaado Docker , se crean una serie de redes por defecto . Para crear una nueva red personal de usuario 


`docker network create nombre_red`





### CREAR DOS CONTENEDORES UNIDOS A DICHA RED



Podemos crear dos contenedores que se unan a la red que se acaba de crear utilizando los siguientes comandos 


`docker run -dit --name contenedor1 --network nombrel_red nombre_imagen `


`docker run -dit --name contenedor2 --network nombre_red nombre_imagen `




### COMPROBAR QUE LOS CONTENEDORES ESTÁN EN LA RED



Para comprobar que los contenedores están en la red podemos emplear la línea de comados 


`docker network inspect nombre_red `


en la sección de contenedores aparecerán los contenedores que están vinculados 




### COMPROBAR QUE LOS CONTENEDORES ESTÁN CONECTADOS ENTRE ELLOS A TRAVÉS DE LA RED




Se puede  utilizar el comando ping para visualizar la conexión entre los contenedores que pertenecen  a la misma red.
Conectamos con la shell del contenedor desde el que queremos hacer el comando ping 



`docker exec -it contenedor1 sh`

 `bash`
 
  
 De esta forma conectamos con al terminal vinculada con el contenedor 
 Ejecutamos un comando ping , debiendo de recibir una respuesta 
 
 
 
### LISTAR LOS CONTENEDORES ASOCIADOS A LA RED



Se puede hacer uso de mismo comando empleado anteriormente 


``docker network inspect nombre_red``




### LISTAR LAS PROPIEDADES DE LA RED



Se puede hacer uso del mismo comando empleado anteriormente 


``docker network inspect nombre_red``



### CREAR OTRA RED 



Se hace uso del mismo comando de creación de redes utilizado anteriormente 


``docker network create nombre_red2``




### LANZAR DOS CONTENEDORES NUEVOS ASOCIADOS A LA NUEVA RED




`docker run -dit --name contenedor3 --network nombrel_red nombre_imagen `


`docker run -dit --name contenedor4 --network nombre_red nombre_imagen `




### COMPROBAR LAS POSIBLES CONEXIONES ENTRE LOS CUATRO CONTENEDORES CREADOS




Si se realiza comando ping entre los contenedores que pertenecen a  la misma red se deberia obtener respuesta . Sin embargo si el inteno de conexion se lleva a cabo entre contenedores pertenecientes a redes diferentes no deberia existir contestación .

Particularmente , empleando las denominaciones usadas en el documento como ejemplo 


- Un ping entre contenedor1 y contenedor2 obtiene respuesta 
- Un ping entre contenedor3 y contenedor4 obtiene respuesta
- Un ping entre contenedor1 u contenedor2  y contenedor3 contenedor4 no obtiene respuesta al pertenecer a redes difentes 
    




### GUIA DE INICIACIÓN DE DOKER COMPOSE




Docker Compose es una herramienta que permite ejecutar y definir aplicaciones multicontenedor en Docker emplenado determinados archivos denominados archivos de configuración en formato YALM.

Teniendo en cuenta el requerimiento específico de la práctica se procede a describir el proceso de creación de un archivo de configuración para la estructura de tres contenedores.


- Instalar Docker Compose

- Crear un archivo _docker-compose.yml_  : mediante la sintaxis YALM se define la estructura de múltiples contenedores requerida

- Definir los servicios : se definen las variables necesarias para la configuración , ya siendo volúmes, contenedores, imágenes utilizadas , etc.

- Inicializar el archivo recién creado: se ejecuta el comando _docker-compose up_ en terminal, iniciándose todos los contenedores definidos en el archivo .La ejecución debe realizarse en el directorio en donde se encuentra el fichero de configuración



### EJEMPLO DE UN ARCHIVO DOCKER-COMPOSE.YML



  version: '3.8'



services:

web:

	image:nginx:latest
	ports:
	 -"80:80"
	 

app:

	image:node:14
	working_dir:/usr/src/app
	volumes:
	 - .:/usr/src/app
	 
	command:npm start
	
db:

	image:postgres:latest
	environment:
	
	  POSTGRES_USER: nombre de usuario 
	  POSTGRES_PASSWORD: contraseña 







Se procede a la explicación general de los comandos empleados para construir nuestro ejemplo de archivo de configuración 




-**Version** : concretiza la version de documento de configuración que se está utilizando, en nuestro caso sera la '3.8'

-**Services**: cada uno de los apartados que se encuantran dentro de esta sección se refieren a cada uno de los contenedores creados para el propósito concreto 

 Cada uno de los servicios tiene un nombre en particular , que posteriormente junto con los nombres de cada contenedor se pueden resolver con el sistema de resolucion de dns de la propia red generada
 
 
-**Image**: hace referencia a al imagen asociada a cada contenedor 
 
-**Ports**: define el mapeo de puertos del contenedor al huesped.
 
-**working_dir**: se define el directorio de trabajo dentro del contenedor particular

-**Volumes**: montaje del directorio actual en el propio contenedor, pudiendo probar cambios a tiempo real 

-**Command**:la línea de código que se ejecutará al iniciar el contenedor asociado

-**Environment**: define variables de entorno necesarias para la configuración en  este caso de la base de datos .








### PARÁMETROS ADCIONALES QUE SE PUDEN UTILIZAR EN UN ARCHIVO DE CONFIGURACIÓN YML.




- **depends on**: establece un nivel de dependencia , generando un orden de iniciación de contenedores . Se generan prioridades de iniciación .


- **restart**: permite definir la política de reinicio para un contenedor específico


- **environment**: se construyen variables de entorno para un contenedor en particular.


- **build**: permite construir imagenes de contenedor a partir de cógigo de fuente local


