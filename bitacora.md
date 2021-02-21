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