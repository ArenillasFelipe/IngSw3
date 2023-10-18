## TP4

1. Descripción de los Contenedores:

Se han instanciado 15 contenedores, cada uno exponiendo un servicio único, tales como catálogo, carritos, órdenes, envíos, pagos, usuarios, entre otros. Destacan servicios de bases de datos MongoDB y MySQL, así como un servicio de colas de mensajes RabbitMQ y su consumidor, queue-master. Además, se cuenta con un servicio de API Gateway denominado edge-router y un front-end.

3. Uso de Repositorios Separados:

La elección de repositorios individuales para cada servicio ofrece la ventaja de independencia y desacoplamiento. En caso de fallos, el impacto se limita al servicio afectado, preservando la integridad del sistema. Facilita el escalado horizontal, permitiendo la creación dinámica de instancias para servicios con alta demanda. También proporciona una abstracción efectiva en las comunicaciones inter-servicios, independientemente del lenguaje utilizado. No obstante, la complejidad inherente a las arquitecturas de microservicios presenta desafíos, como la necesidad de sincronización entre servicios y la gestión de posibles conflictos en recursos compartidos.
Identificación del API Gateway:

4. El contenedor que cumple la función de API Gateway se denomina edge-router.


5. Operación de curl http://localhost/customers:

6. Este comando consulta el servicio de clientes para procesar la operación.
Operaciones de curl http://localhost/catalogue y curl http://localhost/tags:

7. Respectivamente, estas solicitudes apuntan a los servicios de catálogo y etiquetas.
Persistencia de Datos en los Servicios:

8. La persistencia de datos se logra mediante las bases de datos asociadas a cada servicio.
Procesamiento de la Cola de Mensajes:

9. El componente encargado de procesar la cola de mensajes responde al nombre de queue-master, actuando como consumidor.


10. Los microservicios se comunican mediante una interfaz basada en mensajes o colas de mensajes, específicamente utilizando RabbitMQ como plataforma de mensajería.