# Código RAPID

MODULE Module1

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
    ! ... (resto de targets Target_20 a Target_2790)

    ! =========================================================================
    ! 1. CONTROL DE BANDA TRANSPORTADORA
    ! =========================================================================
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

    ! =========================================================================
    ! 2. CONTROL DE ENTRADAS Y SALIDAS DIGITALES (RESET DE SALIDAS)
    ! =========================================================================
    PROC Reset_All_DO()
        Reset DO_01;
        Reset DO_02;
        Reset DO_03;
    ENDPROC

    ! =========================================================================
    ! 3. RETORNO DEL ROBOT A HOME
    ! =========================================================================
    PROC GoTo_Home()
        MoveJ pHome, v200, z50, MyNewTool\WObj:=Workobject_2;
    ENDPROC

    ! =========================================================================
    ! 4. POSE DE MANTENIMIENTO (Activada por DI_02)
    ! =========================================================================
    PROC Modo_Mantenimiento()
        Stop_Conveyor;
        Reset_All_DO;
        Set DO_02; ! Indicador luminoso Testigo 2
        MoveJ pMantenimiento, v100, fine, MyNewTool\WObj:=Workobject_2;
        WaitDI DI_02, 0; ! Espera a que se desactive la señal de mantenimiento
        Reset DO_02;
        GoTo_Home;
    ENDPROC

    ! =========================================================================
    ! 5. RUTINA FARMEAR AURA (Activada por DI_03)
    ! =========================================================================
    PROC Rutina_FarmearAura()
        VAR int i;
        Stop_Conveyor;
        Reset_All_DO;
        Set DO_03; ! Indicador luminoso Testigo 3
        
        ! Secuencia creativa de movimientos articulares y rotación del TCP
        FOR i FROM 1 TO 3 DO
            MoveJ Target_1360, v150, z10, MyNewTool\WObj:=Workobject_2;
            MoveJ Target_2150, v150, z10, MyNewTool\WObj:=Workobject_2;
        ENDFOR
        
        Reset DO_03;
        GoTo_Home;
    ENDPROC

    ! =========================================================================
    ! 6. RUTINA DE DECORACIÓN (Pac-Man y Nombres)
    ! =========================================================================
    PROC Trazado_Decoracion()
        ! Aproximación en Z segura (-10 mm)
        MoveJ Target_360, v100, z5, MyNewTool\WObj:=Workobject_2;
        
        ! Trazado continuo del Pac-Man y letras
        MoveL Target_10, v50, fine, MyNewTool\WObj:=Workobject_2;
        ! ... Se ejecutan las trayectorias de MoveL para nombres Sharon, Esteban, Fabian
        
        ! Elevación final tras completar el dibujo
        MoveJ Target_490, v100, z10, MyNewTool\WObj:=Workobject_2;
    ENDPROC

    ! =========================================================================
    ! 7. CICLO AUTOMÁTICO COMPLETO (Activado por DI_01)
    ! =========================================================================
    PROC Ciclo_Automatico()
        Reset_All_DO;
        
        ! Paso 1: Avance de banda con pastel hacia zona de trabajo
        Move_Conveyor_FWD;
        WaitTime 3.0; ! Simulación de llegada del pastel
        Stop_Conveyor;
        
        ! Paso 2: Ejecución de la decoración
        Set DO_01; ! Indicador luminoso Testigo 1
        Trazado_Decoracion;
        Reset DO_01;
        
        ! Paso 3: Retorno a HOME y evacuación del pastel decorado
        GoTo_Home;
        Move_Conveyor_FWD;
        WaitTime 3.0;
        Stop_Conveyor;
    ENDPROC

    ! =========================================================================
    ! RUTINA PRINCIPAL (BUCLE CONTINUO DE ESCUCHA DE SENALES DI)
    ! =========================================================================
    PROC main()
        GoTo_Home;
        Reset_All_DO;
        
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

ENDMODULE
