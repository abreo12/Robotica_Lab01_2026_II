# Documentación

En esta carpeta se encuentra la documentación correspondiente al desarrollo del Laboratorio No. 01 de Robótica Industrial.

Aquí se incluirán:

* **Descripción de la solución implementada:** Automatización de la celda con el robot ABB IRB 140 e integración con la banda transportadora para la decoración del pastel (nombres de los integrantes y Pac-Man).
* **Diagrama de flujo de la secuencia completa:** Lógica de control para el ciclo automático (`DI_01`), pose de mantenimiento (`DI_02`) y rutina Farmear Aura (`DI_03`).
* **Plano de planta de la celda:** Disposición espacial del robot, la banda transportadora y zonas de trabajo.
* **Descripción de las instrucciones y funciones de RAPID utilizadas:** Uso de `MoveJ`, `MoveL`, `Set`, `Reset`, `WaitTime`, condicionales `IF`, ciclos `FOR`/`WHILE` y manejo de `wobjdata`.

* ## Descripción de Instrucciones y Funciones de RAPID Utilizadas

### Declaraciones de Datos y Estructuras
* **`MODULE ... ENDMODULE`**: Encapsula y organiza todo el código, rutinas, datos y variables del programa en una unidad lógica dentro del controlador RAPID.
* **`PERS tooldata`**: Declara un dato persistente (`MyNewTool`) que almacena las propiedades físicas y geométricas de la herramienta porta-marcador. Define las coordenadas cartesianas del TCP ($X, Y, Z$), la orientación mediante cuaterniones y las características de masa/centro de gravedad.
* **`TASK PERS wobjdata`**: Define el sistema de coordenadas de trabajo (`Workobject_2`). Permite referenciar todas las trayectorias de la torta a un plano específico, facilitando la reutilización del trazado en distintas ubicaciones cuadrantes ( $x(+), y(+)$ y $x(+), y(-)$ ) sin reprogramar los puntos individuales.
* **`CONST robtarget`**: Define constantes de posición cartesiana (`Target_10`, `Target_20`, etc.). Almacenan las coordenadas tridimensionales de aproximación y trazado, la orientación del TCP y la configuración de ejes del robot.

### Instrucciones de Movimiento
* **`MoveJ`**: Realiza movimientos articulares rápidos hacia una posición. Se utiliza para desplazamientos en el aire donde la trayectoria intermedia no requiere ser una línea recta, como aproximaciones iniciales, elevaciones a la z-segura (puntos con $Z = -10\text{ mm}$) o retornos a la posición **HOME**.
* **`MoveL`**: Ejecuta movimientos rectilíneos controlados. Mantiene la punta del marcador en trayectoria recta a velocidad constante, utilizado para el trazado continuo sobre el pastel de los nombres y la figura de Pac-Man.

### Control de Entradas/Salidas y Sincronización
* **`Set`**: Activa una señal de salida digital poniéndola en nivel alto (`1`). Se utiliza para encender las salidas indicadoras (`DO_01`, `DO_02`, `DO_03`) o accionar el movimiento de la banda transportadora (`FWD_Conveyor`).
* **`Reset`**: Desactiva una salida digital poniéndola en nivel bajo (`0`). Se utiliza para apagar las señales indicadoras y aplicar enclavamientos de seguridad que impiden accionar simultáneamente el avance y retroceso de la banda (`FWD_Conveyor` y `BWD_Conveyor`).
* **`WaitTime`**: Detiene la ejecución del código durante un tiempo determinado en segundos. Permite estabilizar la banda transportadora tras enviar el comando de paro antes de que el robot inicie el contacto con el pastel.

### Lógica y Estructuras de Control
* **`PROC ... ENDPROC`**: Delimita procedimientos y subrutinas modulares. Permite segmentar las tareas del programa (como el control de la banda, la rutina de decoración o la rutina *Farmear Aura*) para llamarlas desde el procedimiento principal.
* **`IF ... THEN ... ELSE ... ENDIF`**: Estructura condicional utilizada para conmutar entre los modos de operación evaluando el estado de las entradas digitales (`DI_01`, `DI_02`, `DI_03`) y controlar fallos o límites de alcanzabilidad.
* **`WHILE ... ENDWHILE`**: Bucle condicional que repite un bloque de código mientras la condición sea Verdadera (`TRUE`). Se utiliza en la rutina principal para mantener el sistema escuchando continuamente las señales de entrada.
* **`FOR ... FROM ... TO ... DO ... ENDFOR`**: Ciclo de repetición con contador definido. Se emplea en la secuencia creativa (*Farmear Aura*) para ejecutar bucles iterativos de movimiento durante la coreografía del manipulador.
