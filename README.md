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

> CONFIG SET requirepass Sapia123mysuperpass

Si queremos grabar nuevamente nos dara error

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

==============================================================================================================
DIA 02 - DESARROLLO
==============================================================================================================

REDIS no esta diseñado para grandes almacenes de datos, ya que este consume recursos directos de memoria del servidor

Para matar el proceso de server, una instancia

# redis-cli –p 14000 shutdown

Redis nacio con 4 tipos de datos

STRING 
LIST
SET : Tener cuidado, se puede perder informacion porque si se coloca el mismo tipo de dato chanca el ultimo
HASHES

---- STRING
Los String NO permiten Datos Duplicados en la lista

mset 
multiples seteos

>mset taco:1000 "hola mundo, estan bien" taco:1001 "Estoy feliz"
>mget taco:1000 taco:1001

setex 
el key dura solo un periodo de tiempo definido en segundos

>setex taco:2001 20 "esto expirara en 60 segundos"

petex
el key dura solo un periodo de tiempo definido en milisegundos

ttl
Para ver cuanto tiempo de vida le queda a una clave

INC 

incrementa en 1 el valor de una clave

---- LISTS
Los String SI permiten Datos Duplicados en la lista
El primero en ingresar es el ultimo en la lista

Agregando elementos a la lista
>lpush departamentos "lima" "la libertad" "piura" "loreto"

Agregando un elemento duplicado.
>lpush departamentos "loreto"

obteniendo el total de elementos de la lista.
>lrange departamentos 0 -1

obteniendo los primeros elementos de la lista.
>lrange departamentos 0 1

Agregando un elemento al inicio de la lista
>lpush departamentos "Cerro de Pasco"

obteniendo los elementos de la lista
>lrange departamentos 0 -1

Agregando un elemento al final de la lista
>rpush departamentos "Madre de Dios"

eliminado elementos de la lista. lrem key count value.
#count > 0: Remove elements equal to value moving from head to tail. Elimina la
cantidad de elemtnos(count) que coincide con "value", pero de arriba hacia abajo.
#count < 0: Remove elements equal to value moving from tail to head. Elimina la
cantidad de elemtnos(count) que coincide con "value", pero de abajo hacia abajo.
#count = 0: Remove all elements equal to value.

>lpush departamentos "Cusco" "Lima" "Cusco" "Arequipa" "Cusco"

Si valor es negativo empieza de Abajo hacia Arriba
Si valor es positivo empieza de Arriba hacia Abajo
Si es 0 Borra todos los que encuentra
>lrem departamentos -2 "Cusco"


Obteniendo la cantidad de elementos
>llen departamentos

Obteniendo el primer elemento de la lista.
>lindex departamentos 0

Obteniendo el ultimo elemento de la lista
>lindex departamentos -1

eliminando el primer elemento de la lista
>lpop departamentos

eliminando el ultimo elemento de la lista
>rpop departamentos

liberando memoria
>flushall

Validando liberación
>lrange departamentos 0 -1

---- SET
Ingreso de Datos Aleatorio pero es mas rapido al leer los extremos
Este tipo de dato NO duplica valores en la lista
El primero en ingresar es el primero en la lista

Gestionando los conjuntos(SET) de datos, inserción aleatoria
>sadd departamentos "Piura" "Lima" "Pasco" "Madre de Dios"

listando los miembros de conjunto de datos, se observa el ingreso aleatorio
>smembers departamentos

Eliminando un registro el ordenmiento se reestructura
>srem departamentos "Loreto"

Agregando un nuevo registro, no me permite el ingreso
>sadd departamentos "Loreto"

Obteniendo la cantidad de elementos
>scard departamentos

Pregunatando si un elemento esta en el conjunto de datos 0=no 1=si
>sismember departamentos "Piura"

#Sacando al primer elemento de la lista
>spop departamentos

#elimine todos los elementos del conjunto.
#agregar elementos a la lista
>sadd departamentos01 "Piura" "Lima" "Pasco" "Madre de Dios"
>sadd departamentos02 "Piura" "Lima" "Huanuco"
>sadd departamentos03 "Pasco" "Lima" "Huancayo"
#diferencia departamentos01,departamentos02
>sdiff departamentos01 departamentos02
#intersección de conjuntos set
>sinter departamentos01,departamentos02
#unión de conjuntos set
>sunion departamentos01,departamentos02

---HASHSET


#Usando el HashSet para multiles hash <expresion> key [field(key') value [...]]
>hmset residencia Jose Lima Pedro Piura Miguel Huacho

Esto es tipo un JSON

Residencia {
 Jose Lima
 Pedro Piura
 Miguel Huacho
 }

#Insertando un elemento al hash multiple. hset key field value
hset residencia Wally barranca
#Inserten los mismo datos que pasa?. hset key field value
hset residencia Wally barranca
#Obteniendo el valor de hash <expresion> key field.
>hget residencia Jose
#Obteniendo el valor de hash,lugar de residencia, de todas las campos. hgetall key
>hgetall residencia
#Validando si existe un elemento. hexists key field
>hexists residencia Jose
#Obteniendo los key de todos los hash: hkeys key
>hkeys residencia
#Obteniendo los values de todos los hash: hvals key
>hvals residencia
#liberando memoria
>flushall

---SORTED SET

#usando el zxx para gestionar elementos, cardinalidad y score de elementos en
conjunto ordenado(sorted set).
#zadd key [NX|XX] [CH] [INCR] score member [score member ...]
#creando una hash con score asignados a los elementos. Si tienen el mismo score se
usa el valor ordenando en base al alfabeto
>zadd departamentos 1 "Lima" 1 "Piura" 2 "La Libertad" 3 "Lambayeque"
#listando elementos del conjunto ordeando. zrange key start stop [WITHSCORES]
>zrange departamentos 0 -1
#Listando el primer elemento
>zrange departamento 0 0
#listando elementos con su score
>zrange departamentos 0 -1 WITHSCORES
#mostrando la cardinalidad, cantidad de elementos. zcard key
>zcard departamentos
#Mostrando la cantidad de elementos del conjunto ordenado (sorted set), definiendo
su score. zcount key min max
>zcount departamentos 1 3
#Mostrando la cantidad de elementos con score 1 y 2
>zcount departamentos 1 2

#Mostrando la cantidad de elementos sin incluir los de score 1
>zcount departamentos 2 3
>zcount departamentos (1 3
#mostrando el total de elementos del sorted set. zcount key min max
>zcount departamentos -inf +inf
#aumentar el score a un elemento del conjunto.
#mostramos los elementos con su score
>zrange departamentos 0 -1 WITHSCORES
#Para aumentar el score de uno de los elementos o miembros. zincrby key increment
member
>zincrby departamentos 1 Lima
#Vuelva a listar
>zrange departamentos 0 -1 WITHSCORES
#Buscar la posición actual de un elemento. zrank key member
>zrank departamentos Lima
#Mostrar el score de un elemento o miembro. zscore key member.
>zscore departamentos Lima
#Remover elemento o miembro. zrem key member [member ...]
>zrem departamento Lima












