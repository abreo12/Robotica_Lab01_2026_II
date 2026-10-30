# Herramienta

En esta carpeta se incluirá la información relacionada con la herramienta diseñada para el robot.

Aquí se incluirán:

* **Diseño de la herramienta:** Porta-marcador adaptado al *flange* del robot ABB IRB 140.
* **Modelo CAD:** Archivos de modelado 3D de la herramienta y la superficie del pastel.
* **Archivos del modelo:** Archivos en formato `.stl` / `.sat`.
### Metodología de Calibración y Análisis de Variabilidad (Real vs. Virtual)

El proceso de calibración del TCP en el robot real se realizó de manera iterativa mediante el método de los 4 puntos en el FlexPendant. En las primeras pruebas se obtuvieron errores elevados (entre el 15% y el 7%), causados por aproximaciones desde ángulos muy parecidos y por la imprecisión visual al alinear la herramienta con el cono de referencia. Siguiendo las recomendaciones del docente y del monitor para lograr un error inferior al 5%, la clave consistió en abordar el punto fijo desde cuatro orientaciones espaciales drásticamente disímiles entre sí. Con esto, el controlador IRC5 calculó la intersección geométrica con mayor exactitud, reduciendo el error final a aproximadamente un **4%**, el cual quedó registrado en la variable persistente `PERS tooldata MyNewTool` del código RAPID.

Respecto a la comparación con la simulación, mientras que en RobotStudio es posible definir teóricamente un TCP con un error numérico de cero al basarse en el modelo CAD nominal, dicha calibración ideal no contempla la realidad física del sistema. Como nos explicó el docente, el entorno virtual no considera las variables físicas externas ni la variabilidad ambiental de la celda, tales como tolerancias de la impresión 3D, holguras mecánicas en el acople, imprecisiones de montaje o desalineaciones en la superficie de trabajo. Por esta razón, la calibración realizada directamente en el manipulador físico —a pesar de conservar un pequeño margen de error propio de la medición real— es la única que compensa de forma efectiva las dinámicas del entorno de trabajo, garantizando el trazo correcto sobre el *WorkObject*.

