# Código RAPID

En esta carpeta se almacena el código completo utilizado durante el desarrollo del laboratorio.


### 1. Rutina de decoración

```rapid
PROC Trazado_Decoracion()
    ! Aproximación en Z segura (-10 mm)
    MoveJ Target_360, v100, z5, MyNewTool\WObj:=Workobject_2;
    
    ! Trazado continuo del Pac-Man y los nombres del equipo
    MoveL Target_10, v50, fine, MyNewTool\WObj:=Workobject_2;
    
    ! Elevación final tras completar el dibujo
    MoveJ Target_490, v100, z10, MyNewTool\WObj:=Workobject_2;
ENDPROC
```

---

### 2. Control de la banda transportadora

```rapid
PROC Move_Conveyor_FWD()
    Reset BWD_Conveyor;
    Set FWD_Conveyor;
ENDPROC

PROC Move_Conveyor_BWD()
    Reset FWD_Conveyor;
    Set BWD_Conveyor;
ENDPROC

PROC Stop_Conveyor()
    Reset FWD_Conveyor;
    Reset BWD_Conveyor;
    WaitTime 0.5; ! Estabilización de la banda
ENDPROC
```

---

### 3. Ciclo automático

```rapid
PROC Ciclo_Automatico()
    Reset_All_DO;
    
    ! Paso 1: Avance de banda con pastel hacia zona de trabajo
    Move_Conveyor_FWD;
    WaitTime 3.0;
    Stop_Conveyor;
    
    ! Paso 2: Ejecución de la decoración
    Set DO_01; ! Testigo 1 Activo
    Trazado_Decoracion;
    Reset DO_01;
    
    ! Paso 3: Retorno a HOME y evacuación del pastel decorado
    GoTo_Home;
    Move_Conveyor_FWD;
    WaitTime 3.0;
    Stop_Conveyor;
ENDPROC
```

---

### 4. Pose de mantenimiento

```rapid
PROC Modo_Mantenimiento()
    Stop_Conveyor;
    Reset_All_DO;
    Set DO_02; ! Testigo 2 Activo
    MoveJ pMantenimiento, v100, fine, MyNewTool\WObj:=Workobject_2;
    WaitDI DI_02, 0; ! Espera la desactivación del pulsador
    Reset DO_02;
    GoTo_Home;
ENDPROC
```

---

### 5. Rutina Farmear Aura

```rapid
PROC Rutina_FarmearAura()
    VAR int i;
    Stop_Conveyor;
    Reset_All_DO;
    Set DO_03; ! Testigo 3 Activo
    
    ! Secuencia demostrativa de movimientos
    FOR i FROM 1 TO 3 DO
        MoveJ Target_1360, v150, z10, MyNewTool\WObj:=Workobject_2;
        MoveJ Target_2150, v150, z10, MyNewTool\WObj:=Workobject_2;
    ENDFOR
    
    Reset DO_03;
    GoTo_Home;
ENDPROC
```

---

### 6. Control de las entradas y salidas digitales

```rapid
PROC Reset_All_DO()
    Reset DO_01;
    Reset DO_02;
    Reset DO_03;
ENDPROC

PROC main()
    GoTo_Home;
    Reset_All_DO;
    
    ! Bucle continuo de monitoreo de señales de la botonera (DI)
    WHILE TRUE DO
        IF DI_01 = 1 THEN
            Ciclo_Automatico;
        ELSEIF DI_02 = 1 THEN
            Modo_Mantenimiento;
        ELSEIF DI_03 = 1 THEN
            Rutina_FarmearAura;
        ENDIF
        WaitTime 0.1;
    ENDWHILE
ENDPROC
```

---

### 7. Retorno del robot a HOME

```rapid
PROC GoTo_Home()
    MoveJ pHome, v200, z50, MyNewTool\WObj:=Workobject_2;
ENDPROC
```
