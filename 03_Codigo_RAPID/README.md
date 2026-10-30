# Código RAPID

### 1. Rutina de decoración

```rapid
PROC Trazado_Decoracion()
    ! Aproximación en Z segura (-10 mm)
    MoveJ Target_360, v100, z5, MyNewTool\WObj:=Workobject_2;
    
    ! Trazado continuo del Pac-Man y nombres (Sharon, Esteban, Fabian)
    MoveL Target_10, v50, fine, MyNewTool\WObj:=Workobject_2;
    ! ... (secuencia de trazos lineales sobre la maqueta)
    
    ! Elevación final tras completar el dibujo
    MoveJ Target_490, v100, z10, MyNewTool\WObj:=Workobject_2;
ENDPROC
