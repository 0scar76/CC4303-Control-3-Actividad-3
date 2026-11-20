# Actividad 3 - Control 3 - CC4303 Redes

En esta actividad no se utilizo repositorio en GitHub, ya que todo se realizo en Einar.

**En esta actividad no se utilizo la inteligencia artificial**

## Integrantes:
* **Oscar Conejeros Bruce**
* **Julio Yáñez Tejada**

## Sobre la actividad

En esta actividad, se pidio simular la siguiente red en Einar, mediante el uso de routers BGP:

![Red a simular](red.jpg)

En la imagen, aquellas IP que estan en color negro corresponden a las elegidas por el grupo.

## Respuestas

1. Para esta actividad, se decidio elegir el nombre "manzana" para el grupo.

2. En este ejemplo no hubieron dificultades en la implementacion, ya que las instrucciones estaban bien escructuradas y eran una muy buena guia, pero de todas maneras es interesante poder aprender sobre redes de una forma mas practica y sin riesgo de romper nada.
A continuacion se muestran imagenes del proceso de creacion de la red y la ejecucion inicial, siguiendo los pasos del ejemplo:
![Creacion de la red paso 1](ss_ej1/creacion1_ej.png)
![Creacion de la red paso 2](ss_ej1/creacion2_ej.png)
![Creacion de la red paso 3_1](ss_ej1/creacion3_ej_1.png)
![Creacion de la red paso 3_2](ss_ej1/creacion3_ej_2.png)
![Creacion de la red paso 3_3](ss_ej1/creacion3_ej_3.png)
![Ejecucion sin nada](ss_ej1/ejecutando_ej.png)
Luego, se procedio a hacer pings de prueba y se vieron las rutas actuales para verificar que la red funcione correctamente:
![Pings antes](ss_ej1/ping_antes_ej.png)
![Rutas antes](ss_ej1/rutas_antes_ej.png)
Como inicialmente cada router conoce unicamente a los vecinos, se procede a entregar las rutas y volver a probar las conexiones:
![Pings despues](ss_ej1/ping_despues_ej.png)
![Rutas despues](ss_ej1/rutas_despues_ej.png)
Con esto, finalmente se logro la conexion entre los 2 PC.

3. Este ejemplo fue un poco mas complicado que el anterior, ya que requeria mas conocimientos sobre bgp y la sintaxis y semantica de los comandos para realmente saber que se estaba haciendo. De todas maneras es un ejemplo interesante que ayuda a entender la configuracion de las redes de una forma mas acercada a la realidad.
A continuacion, se muestran las imagenes de la configuracion de los routers bgp:
Primero, se configuran los routers como routers bgp con sus respectivos ASN, indicando cuales son sus vecinos, y se hace ping desde PC1 a PC2:
![Pings primera parte](ss_ej2/ping_antes_bgp.png)
Ademas, se ve la tabla de rutas de R1:
![Rutas primera parte](ss_ej2/rutas_antes_bgp.png)
Luego se crea una nueva red, para luego filtrarla para que R2 no la promocione:
![Nueva red antes del filtro 1](ss_ej2/nueva_red_antes1_bgp.png)
![Nueva red despues del filtro 1](ss_ej2/nueva_red_despues1_bgp.png)
Luego se sigue con el caso que, a una nueva red de R2, es R1 quien no quiere permitir que se utilice:
![Nueva red antes del filtro 2](ss_ej2/nueva_red_antes2_bgp.png)
![Nueva red despues del filtro 2](ss_ej2/nueva_red_despues2_bgp.png)
Concluyendo asi con el ejemplo.

4. Se adjuntan las fotos del proceso de creacion del laboratorio donde se simula la red mostrada al  inicio del informe (nn = ñ, Einar no dejaba usar el caracter exacto):
![Creacion de la red paso 1](ss_act/creacion1_act.png)
![Creacion de la red paso 2_1](ss_act/creacion2_act_1.png)
![Creacion de la red paso 2_2](ss_act/creacion2_act_2.png)
![Creacion de la red paso 2_3](ss_act/creacion2_act_3.png)
![Creacion de la red paso 3_1](ss_act/creacion3_act_1.png)
![Creacion de la red paso 3_2](ss_act/creacion3_act_2.png)
![Creacion de la red paso 3_3](ss_act/creacion3_act_3.png)
![Creacion de la red paso 3_4](ss_act/creacion3_act_4.png)

5. Ahora, se procede a configurar los routers como routers bgp, con sus respectivos ASN, vecinos y direcciones IP:
![Configuracion routers bgp 1](ss_act/parte5_act_1.png)
![Configuracion routers bgp 2](ss_act/parte5_act_2.png)
La configuracion de las distintas IP que usa cada router estan disponibles en la imagen al inicio del documento.

6. Luego, se procede a ejecutar los comandos dados para la actividad:
![Configuracion route map](ss_act/parte6_act.png)
Caso salida_R1: Este route-map permite a R1 llegar solo a la red 192.168.0.0/24 a traves de R0.
Caso salida_R2: En este caso, se hace lo mismo que en salida_R1, pero ademas agregando artificialmente 4 pasos al camino.
R2 preferiria pasar por R1, ya que por el aumento artificial al tamaño del camino, el camino por R1 le queda mas corto.

7. El diagrama de comunicaciones BGP:
![Diagrama BGP](diagrama.jpg)
En R3:
configure terminal 
    ip prefix-list salida_new seq 10 permit 192.168.3.0/24
    router bgp 65003
        neighbor 192.168.10.2 prefix-list salida_new out
        neighbor 192.168.10.3 prefix-list  salida_new out
En R4:
configure terminal 
    ip prefix-list salida_new seq 10 permit 192.168.4.0/24
    router bgp 65004
        neighbor 192.168.10.2 prefix-list salida_new out
        neighbor 192.168.10.3 prefix-list  salida_new out
Estos comandos hacen que, para las ip de R1 y R2, los routers R3 y R4 solo acepten acceso a sus respectivas redes.
Se muestra la ejecucion de los comandos y los resultados de los pings entre los PC:
![Comandos de R3 y R4](ss_act/parte7_antes_act.png)
![Ping de PC](ss_act/parte7_despues_act.png)
Despues de ejecutar estos comandos, vemos que, si hacemos pings entre los PC, aun se pueden comunicar entre ellos.

8. Se ejecuto el comando dado para la actividad y se hizo ping entre los distintos PC:
![Shutdown comunicacion entre R0 y R1](ss_act/parte8_act_1.png)
Luego de ejecutar el comando, vemos que los PC aun pueden comunicarse entre si, pero si usamos el comando show ip bgp en R1, vemos que para llegar a la red 192.168.0.0/24 ahora usa R2 en vez de ir directamente a R0, ya que cerramos ese canal de comunicacion. A continuacion la nueva tabla de rutas de R1:
![Rutas R1](ss_act/parte8_act_2.png)