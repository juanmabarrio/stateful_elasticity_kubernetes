# "Elasticidad stateful en kubernetes"
## Trabajo de fin de máster
El objetivo de este trabajo es ofrecer un estudio que sirva de introducción para alguien que quiera escalar un servicio stateful con kubernetes.

### Tareas: 
1. Definición de términos
* Servicio no stateful vs servicio stateful
* Autoescalado
* Kubernetes. Qué es?
* Service
* Operador

2. Revisión por encima, rapida, de alto nivel sobre como se consigue la elasticidad no stateful, con Deployment, Load Balancer, y el Horizontal Pod Autoscaler para el autoescalado.
3. Qué son y como funcionan los Stateful Sets
4. Estudiar como PostgreSQL y RabbitMQ utilizan stateful sets para implementar scale in y scale out. 
5. Qué soporte tienen las apps para poder implementar scale in y scale out
6. Prueba que permita probar la capacidad, por ejemplo
- simular pruebas de carga de envío de mensajes en la Rabbit y forzar el "scale-out" y después de un tiempo reducir la carga para que se vea el "scale-in" sin caída de servicio. 
- Lo mismo pero con base de datos. Aquí habría que estudiar si se puede hacer dinámico (añadir nodos bajo demanda) o es una configuración que se tiene que definir al arrancar. Pero en cualquier caso se podrían hacer pruebas de carga para ver cómo se comporta el sistema con 1 nodo o 2 nodos, etc...
7. Describir en alto nivel cómo utilizan los operators el recurso StatefulSet para hacer scale-out y scale-in the los servicios. Aquí sería buscar artículos en los que se explicara y si te animas, ver el código fuente de algunos operadores y explicar lo que hace cada uno, intentar sacar algún patrón, compararlos...


-------------------
