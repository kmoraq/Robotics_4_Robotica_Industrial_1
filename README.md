# Robotics_4_Robotica_Industrial_1
Desarollo de la práctica de laboratorio número 4 de la materia de robótica.
## Herramienta
Se decide utilizar un porta herramienta obtenido por impresión 3D, por esto, se realiza un modelo CAD en Invertor del porta herramienta teniendo en cuenta las dimensiones del acople del robot según la hoja de datos proporcionada por el fabricante ABB. 

[![TOOLINVENTOR.jpg](https://i.postimg.cc/NFJKF8hC/TOOLINVENTOR.jpg)](https://postimg.cc/YLFryWCQ)

Adicionalmente, se obtiene de GrabCAD un modelo aproximado a las dimensiones del marcador utilizado para trazar las iniciales de los nombres. Ya con esta información CAD se obtiene de inventor la información del centro de gravedad e inercia proponiendo una masa aproximada de 120g. Finalmente se exporta el modelo en formato sat para que pueda ser utilizado en el sofware Robot Studio.

[![ROBOTOOL.jpg](https://i.postimg.cc/FR0bYBGW/ROBOTOOL.jpg)](https://postimg.cc/r0FRbgBx)

El siguiente fragmento de código de RAPID muestra la declaración de la herramienta tooldata, en esta se define la posición, orientación, centro de gravedad e inercia de la herramienta.

$P_x = -0.503 mm$

$P_y = 0 mm$

$P_z = 175.983 mm$

La orientación es la misma que el el marco de referencia tool0 predefinido en el robot y se proporciona en Quaternios 

$q = [1,0,0,0]$

Las propiedades físicas y del centro de masa son las siguientes:

$m = 120 g$

$C_g =(0.304mm,0mm,59.07mm)$

$Ix=Iz=0.001 kgm^2$

$Iy = 0 kgm^2$
``` RAPID
MODULE Module1
        PERS tooldata Marker:=[TRUE,[[-0.503,0,175.983],[1,0,0,0]],[0.12,[0.304,0,59.07],[1,0,0,0],0.001,0.001,0]];
    PROC main()
        !Add your code here
    ENDPROC
ENDMODULE
```
## Diseño de trayectorias 

### Escritura sobre el plano
Con el fin de determinar las trayectorias con las cuales se crearían las letras, se determinan inicialmente las dimensiones de las letras y así mismo los puntos que las componen, como podemos ver en la imagen a continuación, en donde las dimensiones dadas se encuentran en centímetros. 
[![aln.png](https://i.postimg.cc/RCcbjvCv/aln.png)](https://postimg.cc/pyLZjbnS)

Luego de tener determinados los puntos que componen las letras, procedemos a crear los puntos en el programa de RobotStudio, estos puntos lo insertamos utilizando la herramienta de posición como se muestra en la imagen:

[![pos.png](https://i.postimg.cc/Y0Z96Q4w/pos.png)](https://postimg.cc/QKghs9k6)

Se realiza el posicionamiento de todos los puntos, teniendo en cuenta puntos de aproximación y también de elevación, y se obtiene la ruta mostrada a contiunación:

[![Whats-App-Image-2022-06-29-at-22-05-28.jpg](https://i.postimg.cc/d316kcCx/Whats-App-Image-2022-06-29-at-22-05-28.jpg)](https://postimg.cc/FdM38qDy)

Teniendo los puntos posicionados procedemos a verificar que todos los puntos puedan ser logrados con una misma configuración del robot, pues sino se cuenta con la misma configuración para todos los puntos no podrá seguir la trayectoria como se requiere. Como se encontraron casos en los que algunos puntos no contaban con las mismas configuraciones que los demás, se modificaba la posición del grupo de puntos para lograr la misma configuración en todos los puntos. Se genera la trayectoria seleccionando los puntos en el orden en el que se requiere que sean unidos utilizado la herramienta de ruta que se muestra a continuación.

[![ruta.png](https://i.postimg.cc/BQysMxqj/ruta.png)](https://postimg.cc/56wZtQQJ)

Se genera la trayectoria que se muestra a continuación:

[![Whats-App-Image-2022-06-29-at-22-05-29.jpg](https://i.postimg.cc/6qpw1c2v/Whats-App-Image-2022-06-29-at-22-05-29.jpg)](https://postimg.cc/47qrYz1f)

Siguiente a esto, después de haber probado la simuación en RobotStudio, subimos el código en RAPID mostrado en la siguiente sección y probamos el código en el robot y obtenemos los siguientes resultados probando de dos maneras distintas.
 + Inicialmente probamos siguiendo instrucción por instrucción para verificar que los movimientos estuviesen correctos: https://youtube.com/shorts/sk4Xs8kjrB8?feature=share

 + Después de verificar las instrucciones individuales, utilizando la rutina completa: https://youtube.com/shorts/rI1EoXUxG_k?feature=share
 
### Escritura sobre el plano inclinado a 30°

## Análisis de código en RAPID

El código de Rapid se desarrolla por medio de modulos, dentro de un módulo se desarrollan las rutinas o procedimientos deseados que describen las trayectorias deseadas. También se declaran los robtargets, los work objects y el código de main.
La estructura general de un módulo se declara de la siguiente manera:

``` RAPID
MODULE Module1
  PROC main()
        !Añada aquí su código
    ENDPROC
ENDMODULE
``` 
También se muestra la estructura de un procedimiento, en este caso el main


Se declaran tanto los robtargets (Poses a las que el robot debe llegar y con los cuales se construyen las rutas y trayectorias), como los workobjects (marcos de referencia que ayudan a posicionar los robtargets y a la creación de rutas).

A continuación se muestra el código que muestra la creación de uno de los robtargets, en este caso el llamado aproach, que representa la posición de home inicial de home que toma el robot antes de iniciar la trayectoria. También se muestra el código de creación del workobject llamado Grados, el cuál fue utilizado para posicionar los puntos creados sobre un plano a treinta grados del workobject inicial.

``` RAPID
CONST robtarget aproach:=[[200,450,250],[0,1,0,0],[0,2,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
PERS wobjdata Grados:=[FALSE,TRUE,"",[[270,-370,200],[0.683012702,0.183012702,-0.183012702,-0.683012702]],[[0,0,0],[1,0,0,0]]];
``` 
Nótese que las dos lineas de código mostradas reflejan no solo la posición del robtarget y del workobject, sino también la orientación del marco de referencia que los representa.
Se recalca que estas líneas de código se encuentran dentro de la estructura de MODULE

Luego se muestra el código del procedimiento de una de las rutas desarrolladas. Nótese que inicialmente se declara el tipo de movimiento que realizará el robot. En este caso se utilizaron movimientos del tipo MoveL para el movimiento de aproach que une la posición de home con el punto inicial, por otra parte, se utilizaron movimientos tipo J para unir cada uno de los puntos de las letras de los integrantes del grupo. 

Luego, se declara el punto al que el robot deberá aproximarse, estos puntos fueron previamente creados como se mostro. También se declara el marco de la herramienta y el work object utilizado. Para este caso debido a que la superficie es plana, se utiliza un work object que coincide con el workobject inicial.

Se declaran los valores de velocidad de v100 y los valores de error en z10 de acuerdo a lo declarado en la guía de laboratorio, estas declaraciones también se ven reflejadas en el código.


``` RAPID
    PROC Path_10()
        MoveL aproach,v100,z10,Marker\WObj:=Wob;
        MoveJ Target_10_2,v100,z10,Marker\WObj:=Wob;
        MoveJ Target_20_2,v100,z10,Marker\WObj:=Wob;
        MoveJ Target_30_2,v100,z10,Marker\WObj:=Wob;
        MoveJ Target_40_2,v100,z10,Marker\WObj:=Wob;
        MoveJ Target_50_2_2,v100,z10,Marker\WObj:=Wob;
        MoveJ Target_50_2,v100,z10,Marker\WObj:=Wob;
        MoveJ Target_60_2,v100,z10,Marker\WObj:=Wob;
        MoveJ Target_70_2,v100,z10,Marker\WObj:=Wob;
        MoveJ Target_80_2_2,v100,z10,Marker\WObj:=Wob;
        MoveJ Target_80_2,v100,z10,Marker\WObj:=Wob;
        MoveJ Target_90_2,v100,z10,Marker\WObj:=Wob;
        MoveJ Target_100_2,v100,z10,Marker\WObj:=Wob;
        MoveJ Target_110_2,v100,z10,Marker\WObj:=Wob;
        MoveJ Target_120_2,v100,z10,Marker\WObj:=Wob;
        MoveJ Target_130_2,v100,z10,Marker\WObj:=Wob;
        MoveJ Target_130_2_2,v100,z10,Marker\WObj:=Wob;
        MoveL aproach,v100,z10,Marker\WObj:=Wob;
    ENDPROC
``` 
Para el desarrollo de la aplicación en un plano inclinado se realiza un procedimiento similar, sin embargo, se cambia el workobject por uno creado por el equipo de trabajo. Este workobject se encuentra con la inclinación solicitada de 30°.     

``` RAPID
    PROC Path_20()
        MoveL app,v100,z10,Marker\WObj:=Grados;
        MoveJ Target_10,v100,z10,Marker\WObj:=Grados;
        MoveJ Target_20,v100,z10,Marker\WObj:=Grados;
        MoveJ Target_30,v100,z10,Marker\WObj:=Grados;
        MoveJ Target_40,v100,z10,Marker\WObj:=Grados;
        MoveJ Target_50_4,v100,z10,Marker\WObj:=Grados;
        MoveJ Target_50,v100,z10,Marker\WObj:=Grados;
        MoveJ Target_60,v100,z10,Marker\WObj:=Grados;
        MoveJ Target_70,v100,z10,Marker\WObj:=Grados;
        MoveJ Target_80_3,v100,z10,Marker\WObj:=Grados;
        MoveJ Target_80,v100,z10,Marker\WObj:=Grados;
        MoveJ Target_90,v100,z10,Marker\WObj:=Grados;
        MoveJ Target_100,v100,z10,Marker\WObj:=Grados;
        MoveJ Target_110,v100,z10,Marker\WObj:=Grados;
        MoveJ Target_120,v100,z10,Marker\WObj:=Grados;
        MoveJ Target_130,v100,z10,Marker\WObj:=Grados;
        MoveJ Target_130_3,v100,z10,Marker\WObj:=Grados;
        MoveL app,v100,z10,Marker\WObj:=Grados;
    ENDPROC
``` 
En ambos casos también se especifíca el marco de la herramienta que fue creado, en este caso se le puso el nombre de Marker.
    
