# Docker Swarm Example

Pequeño ejemplo de como hacer un clúster de contenedores en máquinas virtuales (en este caso con virtualbox)

## Dependencias

- [Docker](https://www.docker.com/get-started)
- [Docker Machine](https://docs.docker.com/machine/install-machine/)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

## Instrucciones

1. Primero se necesitará contar con una imagen (puede ser cualquier imagen de docker como por ejemplo una de redis, nginx, etc), para este caso se usará una imagen que tiene un pequeño "hola mundo" desarrollado en express, dicha imagen para este ejemplo se llamará "my-app", para manejar la imagen en las máquinas virtuales se tendrá alojada la imagen en [Dockerhub](https://hub.docker.com/) como [sebasd095/my-app](https://hub.docker.com/r/sebasd095/my-app).
2. Se creará las máquinas virtual en virtualbox a través de dockermachine, para este ejemplo se crearán 3 máquinaas virtuales (1 como nodo manager, 2 como nodos workers):
   #### Manager:
   ![](./img/1.png)
   ### Workers:
   ![](./img/2.png)
3. Verificamos con el comando ls en docker machine que estén corriendo nuestros nodos:
   ![](./img/3.png)
4. Ingresamos al nodo manager con el comando ssh en docker machine:
   ![](./img/4.png)
5. Inicializamos docker swarm en el nodo manager con el comando init --advertise-addr indicandole la ip del nodo manager asi:
   ![](./img/5.png)
6. Con la inicialización anterior se mostrará un comando con el cuál podemos añadir más nodos a swarm, procedemos a ingresar a los demás nodos y pegar y ejecutar dicho comando:
   ![](./img/6.png)
   ![](./img/7.png)
7. Verificamos los nodos que tenemos en el clúster:
   ![](./img/8.png)
8. Ingresaremos al nodo manager y procederemos a crear un servicio con la imagen (sebasd095/my-app) le daremos como nombre al servicio de my-app y le indicamos que queremos solo una réplica:
   ![](./img/9.png)
9. Una vez creado el servicio podemos ver los servicios (en el nodo manager) listados con el comando docker service ls o podemos utilizar el comando inspect para ver con más detalle la información de un servicio en específico:
   ![](./img/10.png)
10. En docker service se pueden ejecutar diversos comandos como:
   ![](./img/11.png)
11. Podemos realizar actualizaciones de los servicios con el comando update, en este caso vamos exponer el puerto 3000 para el servicio de my-app y verificaremos con el comando ls que dichos cambios se hayan hecho:
   ![](./img/12.png)
12. Verificaremos las ip de nuestras máquinas con el comando ls en docker machine (realizar comando fuera de los nodos) y accederemos con el puerto 3000 (asignado en el paso anterior) con cada una de la ip en nuestro navegador, veremos que tendremos acceso al servicio sin importar desde cual nodo accedamos:
   ![](./img/13.png)
13. Adicionalmente podemos ver que hay más comandos disponibles del docker service update (desde el manager)
   ![](./img/14.png)
14. Podremos actualizar por ejemplo el número de replicas del servicio (desde el manager):

   ```bash
   docker service update --replicas 3 my-app
   ```

15. Luego podremos verificar esto con el comando ps en docker service:
   ![](./img/15.png)

De igual modo podremos crear muchos más servicios con las configuraciones que requieran.
