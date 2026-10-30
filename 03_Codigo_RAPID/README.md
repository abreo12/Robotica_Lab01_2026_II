# Código RAPID

```rapid
! =========================================================================
! DECLARACIÓN DE DATOS, HERRAMIENTA Y WORKOBJECT
! =========================================================================
PERS tooldata MyNewTool:=[TRUE,[[94.468,-1.755,194.591],[0.965925826,0,0.258819045,0]],[1,[0,0,1],[1,0,0,0],0,0,0]];
TASK PERS wobjdata Workobject_2:=[FALSE,TRUE,"",[[0,0,72],[1,0,0,0]],[[685.24,-91.428,100],[0,0,1,0]]];

! Puntos clave de posición
CONST robtarget pHome:=[[300,0,400],[1,0,0,0],[0,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
CONST robtarget pMantenimiento:=[[250,150,350],[1,0,0,0],[0,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];

! Puntos de trayectorias (Puntos de trazado sobre la superficie del pastel)
CONST robtarget Target_10:=[[157.117,48.235,0],[1,0,0,0],[0,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
CONST robtarget Target_360:=[[158.106,51.606,-10],[1,0,0,0],[0,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];

! =========================================================================
! 1. CONTROL DE BANDA TRANSPORTADORA
! =========================================================================
PROC Move_Conveyor_FWD()
    Reset BWD_Conveyor;
    Set FWD_Conveyor;
ENDPROC
```
