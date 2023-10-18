# TP3: Arquitectura de Sistemas Distribuidos

## 1. Sistema distribuido simple

Puertos abiertos:
5000 para la aplicación web.
6379 para redis.

Comandos utilizados:
`docker ps` para ver el estado de los contenedores y ver en que puertos están corriendo.
`docker network ls` para ver el listado de las redes que tengo.
`docker network inspect <nombre de la red>` para ver el detalle de la red mybridge.


## 2. Analisis del sistema

El sistema realiza las siguientes acciones:

Crea una instancia de una aplicación Flask y la almacena en la variable app.
Establece una conexión con una base de datos redis, guardando la conexión en la variable redis.
Recupera el valor de la variable de entorno BIND_PORT y lo asigna a la variable bind_port.
Utiliza un decorador para asociar la ruta de destino a una nueva función.
Define la función hello(), que, al ejecutarse, incrementa en uno el valor de la clave hits en redis. Posteriormente, obtiene el valor actual de esa clave y lo devuelve junto con un mensaje en formato de cadena.
Si el script se ejecuta como programa principal, se inicia la aplicación web con un host de 0.0.0.0, permitiendo el acceso desde cualquier IP, y se establece en el puerto especificado por la variable bind_port.
Respecto a la presencia de los parámetros -e en el segundo comando docker run del ejercicio 1, estos se emplean para asignar valores a las variables de entorno de la imagen de Docker.

Si ejecutas docker rm -f web para eliminar el contenedor de la aplicación web y luego ejecutas el comando docker run -d --net mybridge -e REDIS_HOST=db -e REDIS_PORT=6379 -p 5000:5000 --name web alexisfr/flask-app:latest, se crea un nuevo contenedor utilizando la imagen ya descargada.

Eliminar el contenedor de Redis con docker rm -f db resultará en una excepción de error de conexión al cargar la página web.

Si decides restablecer el contenedor de Redis con docker run -d --net mybridge --name db redis:alpine, la página web volverá a funcionar, pero se perderá el recuento de visitas acumulado.

Para evitar la pérdida del recuento de visitas, una solución sería emplear un volumen para almacenar esa información, incluso si se elimina y recrea el contenedor de Redis.


## 3. Utilizando docker compose

Al emplear Docker Compose y ejecutar el comando docker-compose up -d, se lleva a cabo la ejecución del script definido en el archivo docker-compose.yaml. De manera detallada, este procedimiento implica:
La creación de dos servicios o contenedores denominados app y db. El primero se instanciará a partir de la imagen más reciente de la aplicación web mencionada previamente, mientras que el segundo se generará utilizando la imagen más reciente de Redis disponible.
El servicio app se configura con las variables de entorno necesarias y se mapea el puerto 5000 tanto en el host como en el contenedor.
Se establece un volumen para el servicio db, permitiendo la persistencia de datos incluso si el contenedor se elimina. Este volumen se asigna al servicio para garantizar la retención de la información.

## 4. Aumentando la complejidad, analisis de otro sistema distribuido

Este conjunto abarca 6 servicios distintos:

vote: La aplicación de votación desarrollada en Python, la cual expone su contenedor en el puerto 80, conectándolo al puerto 5000 del host.
result: Una aplicación que presenta los resultados de la votación. Su contenedor expone los puertos 80 y 5858, vinculándolos respectivamente a los puertos 5001 y 5858 del host.
redis: Una base de datos Redis que utiliza un volumen para los healthchecks. Su contenedor expone el puerto 6379, mapeándolo al mismo puerto en el host.
db: Una base de datos PostgreSQL que configura variables de entorno, exponiendo el puerto 5432 y vinculándolo al mismo puerto en el host. Emplea un volumen para los datos y otro para los healthchecks.
worker: Un trabajador que requiere que Redis y la base de datos hayan superado los healthchecks antes de iniciar.
seed: Un servicio que solo se activa si se especifica con un comando.

