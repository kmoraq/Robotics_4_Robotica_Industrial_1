# Robotics_4_Robotica_Industrial_1
Laboratorio 4 - Rob ́otica Industrial No. 1
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
        MoveL aproach,v100,z10,tool0\WObj:=Wob;
        MoveJ Target_10_2,v100,z10,tool0\WObj:=Wob;
        MoveJ Target_20_2,v100,z10,tool0\WObj:=Wob;
        MoveJ Target_30_2,v100,z10,tool0\WObj:=Wob;
        MoveJ Target_40_2,v100,z10,tool0\WObj:=Wob;
        MoveJ Target_50_2_2,v100,z10,tool0\WObj:=Wob;
        MoveJ Target_50_2,v100,z10,tool0\WObj:=Wob;
        MoveJ Target_60_2,v100,z10,tool0\WObj:=Wob;
        MoveJ Target_70_2,v100,z10,tool0\WObj:=Wob;
        MoveJ Target_80_2_2,v100,z10,tool0\WObj:=Wob;
        MoveJ Target_80_2,v100,z10,tool0\WObj:=Wob;
        MoveJ Target_90_2,v100,z10,tool0\WObj:=Wob;
        MoveJ Target_100_2,v100,z10,tool0\WObj:=Wob;
        MoveJ Target_110_2,v100,z10,tool0\WObj:=Wob;
        MoveJ Target_120_2,v100,z10,tool0\WObj:=Wob;
        MoveJ Target_130_2,v100,z10,tool0\WObj:=Wob;
        MoveJ Target_130_2_2,v100,z10,tool0\WObj:=Wob;
        MoveL aproach,v100,z10,tool0\WObj:=Wob;
    ENDPROC
``` 
Para el desarrollo de la aplicación en un plano inclinado se realiza un procedimiento similar, sin embargo, se cambia el workobject por uno creado por el equipo de trabajo. Este workobject se encuentra con la inclinación solicitada de 30°.     

``` RAPID
    PROC Path_20()
        MoveL app,v100,z10,tool0\WObj:=Grados;
        MoveJ Target_10,v100,z10,tool0\WObj:=Grados;
        MoveJ Target_20,v100,z10,tool0\WObj:=Grados;
        MoveJ Target_30,v100,z10,tool0\WObj:=Grados;
        MoveJ Target_40,v100,z10,tool0\WObj:=Grados;
        MoveJ Target_50_4,v100,z10,tool0\WObj:=Grados;
        MoveJ Target_50,v100,z10,tool0\WObj:=Grados;
        MoveJ Target_60,v100,z10,tool0\WObj:=Grados;
        MoveJ Target_70,v100,z10,tool0\WObj:=Grados;
        MoveJ Target_80_3,v100,z10,tool0\WObj:=Grados;
        MoveJ Target_80,v100,z10,tool0\WObj:=Grados;
        MoveJ Target_90,v100,z10,tool0\WObj:=Grados;
        MoveJ Target_100,v100,z10,tool0\WObj:=Grados;
        MoveJ Target_110,v100,z10,tool0\WObj:=Grados;
        MoveJ Target_120,v100,z10,tool0\WObj:=Grados;
        MoveJ Target_130,v100,z10,tool0\WObj:=Grados;
        MoveJ Target_130_3,v100,z10,tool0\WObj:=Grados;
        MoveL app,v100,z10,tool0\WObj:=Grados;
    ENDPROC
``` 
    
    
