# bitácora

**dia 1**. Empiezo el proyecto, me baso en el mail de Mica para definir las tareas a completar para el proyecto. En el correo me ofrece una estructura bastante clara de como puede ser  el proyecto: 

Objetivo del proyecto: Una introducción para alguien que quiera aprovechar las features de escabilidad de kubernetes en una apliación stateful. Para ello, describir qué ofrece Kubernetes, estudiar sus operadores, y hacer alguna prueba en la que ponga en funcionamiento estas features.

Tareas a llevar a cabo para completarlo:

* Estudiar cómo funcionan los Stateful Sets
* Conocer cómo los operadores de las bases de datos y colas usan los stateful sets para implementar el scale-in y el scale-out. Qué soporte tienen las apps para poder implementar el scale-in y scale-out.

* Hacer alguna prueba empírica que permita probar esa capacidad. Por ejemplo:

	- simular pruebas de carga de envío de mensajes en la Rabbit y forzar el "scale-out" y después de un tiempo reducir la carga para que se vea el "scale-in" sin caída de servicio. 
	- Lo mismo pero con base de datos. Aquí habría que estudiar si se puede hacer dinámico (añadir nodos bajo demanda) o es una configuración que se tiene que definir al arrancar. Pero en cualquier caso se podrían hacer pruebas de carga para ver cómo se comporta el sistema con 1 nodo o 2 nodos, etc...

* Describir en alto nivel cómo utilizan los operators el recurso StatefulSet para hacer scale-out y scale-in the los servicios. Aquí sería buscar artículos en los que se explicara y si te animas, ver el código fuente de algunos operadores y explicar lo que hace cada uno, intentar sacar algún patrón, compararlos...

Defino las tarea en el Readme.md y empiezo con la primera que me he puesto: "Definición de términos"


--------------------

***dia 2*** Continuo con la segunda tarea, una revision rapida de la escalabilidad SIN estado. La intencion es terminarla rapidito y ponerme ya con cosas de la escalabilidad CON estado.
Empezamos...
-- Ya tengo algo... voy a pasar a algo más interesante, mterme con los Stateful sets.

Leo este articulo de un blog:

3 Ways to Master Stateful Apps in Kubernetes
https://www.cockroachlabs.com/blog/kubernetes-orchestrate-sql-with-cockroachdb/ 

El artículo epxlica el reto que supone las aplicaciones con estado Kubernetes:
La magia de la orequestación de Kubernetes, esto es, ser capaz de rapidamente intercambiar y actualiar conctenedores al vuelo es un gra obstaculo para las aplicaciones con estado y bases de datos. La premisa de la contanerización, el ser capaz de mover y manejar rapida y libremente, se viene completamnete abajo cuando una aplicación de bsase de datos está encadenada a almacenamiento local.

Por qué? Porque los Kubernetes deployments ganan a través de la replicación. Asi es como los Devops pueden construir, desplegar, escalar y desescalar con poco esfuerzo y mayor confianza. Pero la replicación no funciona con apps stateful:

* Las replicas de base de datos no son intercambiables como otros contenedores porque tienen estados unicos.
* Las base de datos necesitan una coordinación estrecha con otros nodos corriendo la misma aplicación para asegurar la versión de los datos.

Asi que el artículo sugiere 3 caminos para el éxito a la hora de manejar aplicaciones con estado en kubernetes.
1. Correr fuera de Kubernetes :)
2. Usar cloud services obstaculo
3. Correr Kubernetes nativo.

## Correr fuera de kubernetes
Es la solución más directa para, arranca una VM y corre la base de datos fuera del entorno de Kubernetes. Desgraciadamente el coste está en que el coste operacional aumenta la tener que duplicar lo que ya ofrece kubernetes de forma automática:
* process monitoring 
* configuration management 
* in-datacenter load balancing 
* service discovery 
* monitoring and logging
Resultado: Máxima capacidad de lección, pero una pila completa de herramientas para manejar fuera de Kubernetes.

## Menos control, menos esfuerzo: correr la bbdd via servicios cloud
Puedes aprovecharte de servicios cloud para menajr tu base de datos fuera de Kubernetes. Lo malo es que te quedas enganchado a este proveedor de serivcios en la nube, lo cual no tiene mucho sentido para aquellos corriendo sus servicios in house o on premises. Ademá

## Kubernetes nativo. Control nativo, minimo esfuerzo. Correr tu bbdd en kubernetes.

Kubernetes tiene dos controladores nativos ya integrados para correr bases de datos dentro de un contenedor, de la misma forma que los deployment funcionan con las applicaciones stateless. Maximizan la integración y la automatización, mantienen mas control sobre la carga de trabajo y eliminan el coste, tiempo y complejidad de mantener un stack separado alrededor de los servicios de base de datos.

### El controlador StatefulSet

Como el Deployment,  un StatefulSet maneja pods que estan basados en una especificacion de contendor idénticas pero que no son intercambiables. Asignan a cada pod un **id único y persistente** (by way of an easy to build HEadless Service) ambas aplicación y base de datos mantienen la conexión independientemente del nodo al que estén asignadas.

También significa que a medida que una aplicacion se escala arriba y abajo, las conexiones se mantienen y se alcanza la persistencia. Esto los hace ideales para aplicaciones que necesitan almacenamiento persistente y estable, además de scaling y updates ordenados y automaticos. Esto ingluye controladores distribuidos como Zookeper asi como clusters MySql, Redis, Kafka, MongoDB y otros. 

To learn more about how StatefulSet supports local storage by way of LocalPersistentVolume, read here. https://kubernetes.io/docs/concepts/storage/volumes/#local

### El controlador DaemonSet

Si los StatefulSets usaban IDs unicos para mantener aplicacion y base de datos conectadas entre nodos, los DaemonSets aseguran que todos(o alggunos) nodos corren una copia de un pod. Si se añade un nodo, también se añade su necesario nodo de base datos. 

Como el nombre indica, el controlador DameonSet es especialmente util cuando estás corriendo procesos backgorund (daemons), especialmente al rededor del monitoring de rendimiento o captura de logs y analisis,.

Al restringir los nodos a apoyo a base de datos, los Daemon Sets eliminan los potenciales problemas de rendimiento de los STatetulSets originados por la contención de recursos y competición.


## Eligiendo el controlador adecuado
how t
La naturaleza de la carga de trabajo y la adherencia a otras mejroes prácticas de Kubernetes deben guiar la elección del controlador stateful de Kubernetes. Aplicaciones de base de datos transaccionales com PostgreSQL son ideales para el más ágil StatefulSet, mientras que procesos backgroud suelen encajar mejor con los DaemonSets.