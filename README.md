<h1 align="center"> Laboratorio 3 Robótica industrial </h1>

## Universidad Nacional de Colombia
-------------------------------------------------------------
> ## Autores

  > - Carlos Mario Jiménez Novoa. [cajimn](https://github.com/cajimn)
  > - Ivanna Lisette Medina Cruz. [IvannaMedinac](https://github.com/IvannaMedinaC)
  > - Juan Sebastian Rangel Araque. [juanBananin](https://github.com/juanBananin)


## Procedimiento

> ## MATLAB

Para realizar la conexión con MATLAB se descargó el toolbox MATLAB ROS, luego se utilizaron las sigiuentes líneas de código: `roscore` en la primera terminal y `rosrun turtlesim turtlesim_node` en la segunda terminal, con esto ya se inicia el programa de turtlesim. 

![mat1](https://user-images.githubusercontent.com/51938754/191159316-b83342ee-dd84-4bd4-90d6-4a18f8050d39.png)


Para saber cuales son los nodes, topics y services del programa se introducen las siguientes líneas de código en un tercer terminal .


![mat5](https://user-images.githubusercontent.com/51938754/191159503-aa21de05-656b-44d0-a76c-2ba889d03193.png)

Primero se va a correr el programa [pruebatortuga](scripts/pruebatortuga.m) para obervar el resultado del movimiento de la tortuga haciendo uso del topic `turtle1/cmd_vel` y su respectivo publisher para enviar información, subscriber para recibirla y por ende cambiar el estado del topic.

![mat2](https://user-images.githubusercontent.com/51938754/191160162-ba412058-6894-47cb-995d-ac61213d503c.png)

Se observa que la tortuga se movió en X como se escribió en el código.

Luego, se procede a realizar la implementación del subscriber para el tópico `turtle1/pose`; para crear el subscriber se reciben dos argumentos: el tópico del cual se va a recibir información y el tipo de mensaje a recibir, en este caso se puede verificar el tipo de mensaje con el código.


![mat6](https://user-images.githubusercontent.com/51938754/191161123-b324b6dd-780c-4a07-8e67-0fa56981cceb.png)

Se observa la creación del subscriber y se accede al ùtimo mensaje con el comando `LatestMessage`

![mat7](https://user-images.githubusercontent.com/51938754/191161652-140778c3-5e53-4214-9584-b28b71f4dbed.png)

El tópico `turtle1/pose` solo tiene la posibilidad de suscribirse y por lo tanto no se le puede enviar información para mover la tortuga mediante un publisher. Por ende, se encuentra la opción de usar el servicio `/turtle1/teleport_absolute`, los servicios no funcionan mediante un subscriber y un publisher, estos funcionan por comunicación petición-respuesta, donde el cliente manda una petición de mensaje y espera la respuesta a través del servidor. 
El código completo se encuentra en [prueba2](scripts/prueba2.m) :

![mat3](https://user-images.githubusercontent.com/51938754/191162621-5d9412f9-5b0c-4b62-9c26-a7d61022b4e7.png)

La función `rossvclient` recibe como argumento el servicio, en este caso `/turtle1/teleport_absolute`. Se espera la respuesta del servidor con el comando `waitForServer(turtleTel)` y se crea el mensaje con `rosmessage`, el cual tiene varios argumentos: distancia en X, en Y y el ángulo de giro de la tortuga, estos son los que se manipulan para mover la tortuga a una posición deseada. Por último para que la tortuga se mueva, se usa la función `call` con los argumentos correspondientes de cliente y mensaje.

Se realiza otras prueba con valores distintos de X, Y y theta.
![mat4](https://user-images.githubusercontent.com/51938754/191163516-4f21a09c-a2ba-40ee-9afb-7b415092e7a5.png)
![mat9](https://user-images.githubusercontent.com/51938754/191165817-4fb8efaf-7625-448a-8c2a-1b3981ac5d6d.png)

Para finalizar el modo maestro se usa el comando `rosshutdown`.
> ## Python 
Para realizar el procedimiento en Python es necesario primero crear un archivo Python que ejecute o lea las teclas que se desean oprimir, en este caso como lo pide la guía se llama myTeleopKey, este se encarga de detectar las teclas que se oprimen, w,s,a y d para los desplazamientos lineales y angulares, space y r para reiniciar la posición de la tortuga o realizar giros de 180.(Ver código adjunto en carpeta scripts). Este archivo se guarda en la carpeta scripts de la carpeta hello_turtle de catkin_ws. 
![Lab21](https://user-images.githubusercontent.com/52113892/191166139-914db828-5a7a-4d1d-b2a9-68707c41847b.png)

Si se tiene configurado el catkin como lo estipula la guía, lo siguiente es modificar el archivo makeList, el cual contiene la lista de los códigos Python que se pueden ejecutar con ros, en ese se coloca el nuevo archivo creado de igual forma que están los otros archivos predeterminados. 


![lab22](https://user-images.githubusercontent.com/52113892/191166248-a68ba035-bab4-4506-a76e-23309a25d159.png)
Se realiza el código de build, dependiendo si se tiene instalado el tools de catkim, se ejecutará uno u otro código determinado por la guía. 

El paso siguiente sería ejecutar el código cargado, para ello se hace:
Primer terminal:
`Roscore`
Segundo Terminal:
`rosrun turtlesim turtlesim_node`

Tercer Terminal:
`source devel/setup.bash`
`rosrun hello_turtle myTeleopKey.py`

Con estos códigos debería funcionar el programa como se ve en las imagenes acontinuación.
![LAB23](https://user-images.githubusercontent.com/52113892/191166960-2a3d86ea-3f3c-4a2d-a2ad-f4fe1e1c1672.png)


https://user-images.githubusercontent.com/68668422/194212007-2602820f-b9a7-4833-accd-4bd9cd6b4dfd.mp4

