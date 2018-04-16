# redis

CURSO DE REDIS 2018
====================

PROFESOR WALTER GONZALES

BD no relacional para un acceso rapido de informacion

Twitter usa redis para el acceso rapido a la informacion

Sunat y Temas financiero nivel nacional que usan este producto por el tema de la concurrencia.

**Credenciales**
--------------------------------

linux - root
pass - redhat

jboss
jboss2018$

http://192.168.1.7/redis

BD no SQL no relacional para acceso rapido.

Se complementa con la BD relacional.

Tiene un Almacenamiento en Memoria y un Almacenamiento Fisico

El de Arquitectura va a decidir en que momento lo baja a disco o en que momento lo tiene en memoria

La Aplicacion puede decidir donde ir a buscar la informacion.

Se puede hacer un proceso de Replicacion donde la informacion llega al Master y este lo Replica a los Slaves

Tambien existe la configuracion de CLuster donde la informacion esta replicada en los demas nodos y se puede configurar para que antes que se caiga el nodo este envie su informacion a los demas nodos

Redis da la particularidad para manejar instancias como jboos

La escritura entra por el Master pero la Lectura puede ser desde el Master o el Esclavo

Es tipo Clave = Valor

**Instalacion**
----------------------------------------------

Credenciales para acceder a la plataforma poner

root@redis.com
redhat

La consola grafica te permite ver todas las configuraciones generales de instalacion

**Comandos**
------------------------------------------------

Para ingresar a la consola cliente

redis-cli -p 14000

Dentro de la consola si lanzamos un ping, debe responder pong

> ping
PONG

Muestra informacion del servidor

> info
> info CPU
  
Para sali de la consola

> quit

Este Almacenamiento esta en memoria y pude vivir mientras la conexion del cliente este activa
Una vez al salir del cliente la informacion se pierde. Esto solo pasa cuando la BD esta en memoria.
Al hacer la instalacion de persistencia los datos se mantienen asi salgas de la sesion.

Para setear una clave valor (x = 1000)

> set x 1000

Para obtener el valor

> get x

Para limpiar la sesion

> FLUSHALL

Para limpiar la consola

> clear

Fuera del cliente para ver los datos del servidor y levantar una instancia por defecto

# redis-server

Para matarla CTRL + C

Para crear una instancia especifica 

# redis-server --port 14001

Para conectarte a la instancia nueva

# redis-cli -p 14001

Cuando no se especifica el puesto redis crea una instancia por defecto en la 6379

Para matar una instancia por defecto

# redis-cli shutdown

Para matar una instancia especifica

# redis-cli -p 14001 shutdown

Se puede levantar n instancias pero va a depender de la performance del server

Para configurar las cosas en vuelo, dentro de la consola cliente:
usamos config set. para hacerlos de manera permanente podemos configurar el archivo de configuracion

Colocamos un pass a la instancia para que nos permita grabar

> CONFIG SET requirepass mysuperpass

Pi queremos grabar nuevamente nos dara error

Para autenticamos usamos

> auth mysuperpass

Para habilitar un Listener de Escucha para monitorear la instancia usar el comenado

> monitor

#Para conseguir la información del log ejecutemos el siguiente comando.

$redis-cli –p 6380

> SLOWLOG GET

#Para ver el tamaño dl log

> SLOWLOG LEN

#Para el reset de la información gestionada por el log

> SLOWLOG reset

Creacion de una arquitectura de Maestro y Esclavo
-------------------------------------------------

Creamos 2 instancias

> redis-server --port 6380 --dbfilename db1.db
> redis-server --port 6381 --dbfilename db2.db

Nos conectamos por un cliente al server 6381

> redis-cli -p 6381

Luego le indicamos que este sera el esclavo del 6380

>  SLAVEOF localhost 6380

Esta accion debe mostrar unas salidas en las ventanas de las instancias 6380 y 6381

Sacando de la regla de juego a nuestro esclavo.. desde el mismo esclavo podemos romper la conexion. 
Nos conectamos a nuestro esclavo

> SLAVEOF NO ONE




























