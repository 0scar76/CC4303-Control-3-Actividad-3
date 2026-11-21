# Actividad 3 - Control 3 - CC4303 Redes

[Repositorio con las imágenes en GitHub.](https://github.com/0scar76/CC4303-Control-3-Actividad-3.git)

**En esta actividad no se utilizó la inteligencia artificial.**

## Integrantes:
* **Oscar Conejeros Bruce**
* **Julio Yáñez Tejada**

## Sobre la actividad

En esta actividad, se pidió simular la siguiente red en Einar, mediante el uso de routers BGP. La topología de la red es la siguiente:

![Red a simular](red.jpg)

En la imagen, aquellas IP que están en color negro corresponden a las elegidas por el grupo. Las de color gris corresponden a las que estaban en el enunciado.

## Respuestas

### Ejemplo: Configurando rutas con Einar

Para esta actividad, se decidió elegir el nombre "manzana" para el grupo.

En este ejemplo no hubo dificultades en la implementación, ya que las instrucciones estaban bien estructuradas y eran una muy buena guía, pero de todas maneras es interesante poder aprender sobre redes de una forma más acercada a la realidad y sin riesgo de romper nada.

A continuación se muestran imágenes del proceso de creación de la red y la ejecución inicial, siguiendo los pasos del ejemplo:

---
![Creación de la red, paso 1](ss_ej1/creacion1_ej.png)
![Creación de la red, paso 2](ss_ej1/creacion2_ej.png)
![Creación de la red, paso 3_1](ss_ej1/creacion3_ej_1.png)
![Creación de la red, paso 3_2](ss_ej1/creacion3_ej_2.png)
![Creación de la red, paso 3_3](ss_ej1/creacion3_ej_3.png)
![Ejecución sin nada](ss_ej1/ejecutando_ej.png)
---

Luego, se procedió a hacer pings de prueba y se vieron las rutas actuales para verificar que la red funcione correctamente:

---
![Pings antes](ss_ej1/ping_antes_ej.png)
![Rutas antes](ss_ej1/rutas_antes_ej.png)
---

Como inicialmente cada router conoce únicamente a los vecinos, se procede a entregar las rutas para completar la red y se vuelven a probar las conexiones:

---
![Pings después](ss_ej1/ping_despues_ej.png)
![Rutas después](ss_ej1/rutas_despues_ej.png)
---

Con esto, finalmente se logró la conexión entre los 2 PC.

### Ejemplo: BGP con Einar

Este ejemplo fue un poco más complicado que el anterior, ya que requería más conocimientos sobre BGP y la sintaxis y semántica de los comandos para realmente saber qué se estaba haciendo. De todas maneras, es un ejemplo interesante que ayuda a entender la configuración de las redes de una forma más cercana a la realidad.
   
Respecto al proceso de creación y configuración de la red BGP. Primero, se configuran los routers como routers BGP con sus respectivos ASN, indicando cuáles son sus vecinos, y se hace ping desde PC1 a PC2:

---
![Pings, primera parte](ss_ej2/ping_antes_bgp.png)
---

Además, se ve la tabla de rutas de R1:

---
![Rutas, primera parte](ss_ej2/rutas_antes_bgp.png)
---

Luego se crea una nueva red, para luego filtrarla para que R2 no la promocione:

---
![Nueva red antes del filtro 1](ss_ej2/nueva_red_antes1_bgp.png)
![Nueva red después del filtro 1](ss_ej2/nueva_red_despues1_bgp.png)
---

Luego se sigue con el caso que, a una nueva red de R2, es R1 quien no quiere permitir que se utilice:

---
![Nueva red antes del filtro 2](ss_ej2/nueva_red_antes2_bgp.png)
![Nueva red después del filtro 2](ss_ej2/nueva_red_despues2_bgp.png)
---

Concluyendo así con el ejemplo.

### Red propuesta

Se adjuntan las fotos del proceso de creación del laboratorio donde se simula la red mostrada al  inicio del informe (nn = ñ):

--
![Creacion de la red paso 1](ss_act/creacion1_act.png)
![Creacion de la red paso 2_1](ss_act/creacion2_act_1.png)
![Creacion de la red paso 2_2](ss_act/creacion2_act_2.png)
![Creacion de la red paso 2_3](ss_act/creacion2_act_3.png)
![Creacion de la red paso 3_1](ss_act/creacion3_act_1.png)
![Creacion de la red paso 3_2](ss_act/creacion3_act_2.png)
![Creacion de la red paso 3_3](ss_act/creacion3_act_3.png)
![Creacion de la red paso 3_4](ss_act/creacion3_act_4.png)
--

Ahora, se procede a configurar los routers como routers bgp, con sus respectivos ASN, vecinos y direcciones IP:

---
![Configuracion routers bgp 1](ss_act/parte5_act_1.png)
![Configuracion routers bgp 2](ss_act/parte5_act_2.png)
---

La configuracion de las distintas IP que usa cada router estan disponibles en la imagen al inicio del documento.

Luego, se procede a ejecutar los comandos dados para la actividad:

---
![Configuracion route map](ss_act/parte6_act.png)
---

Cada uno de estos route-maps actúa de manera ligeramente diferente:

- Caso route-map salida_R1: Este route-map permite a R1 llegar solo a la red 192.168.0.0/24 a traves de R0.
- Caso route-mapsalida_R2: En este caso, se hace lo mismo que en salida_R1, pero ademas agregando artificialmente 4 pasos al camino.

R2 preferiria pasar por R1, ya que por el aumento artificial al tamaño del camino, el camino por R1 le queda mas corto.

El diagrama de comunicaciones BGP:

---
![Diagrama BGP](diagrama.jpg)
---

Los comandos para solucionar lo causado tras cambiar la configuración del router R0 son,en R3:
```
configure terminal 
    ip prefix-list salida_new seq 10 permit 192.168.3.0/24
    router bgp 65003
        neighbor 192.168.10.2 prefix-list salida_new out
        neighbor 192.168.10.3 prefix-list  salida_new out
```
Y en R4:
```
configure terminal 
    ip prefix-list salida_new seq 10 permit 192.168.4.0/24
    router bgp 65004
        neighbor 192.168.10.2 prefix-list salida_new out
        neighbor 192.168.10.3 prefix-list  salida_new out
```
Estos comandos hacen que, para las ip de R1 y R2, los routers R3 y R4 solo acepten acceso a sus respectivas redes.

Se muestra la ejecucion de los comandos y los resultados de los pings entre los PC:

---
![Comandos de R3 y R4](ss_act/parte7_antes_act.png)
![Ping de PC](ss_act/parte7_despues_act.png)
---

Despues de ejecutar estos comandos, vemos que, si hacemos pings entre los PC, aun se pueden comunicar entre ellos.

Finalmente, se ejecutó el comando dado para la parte 8 de la actividad y se hizo ping entre los distintos PC:

---
![Shutdown comunicacion entre R0 y R1](ss_act/parte8_act_1.png)
---

Luego de ejecutar el comando, vemos que los PC aun pueden comunicarse entre si, pero si usamos el comando show ip bgp en R1, vemos que para llegar a la red 192.168.0.0/24 ahora usa R2 en vez de ir directamente a R0, ya que cerramos ese canal de comunicacion. A continuacion la nueva tabla de rutas de R1:

---
![Rutas R1](ss_act/parte8_act_2.png)
