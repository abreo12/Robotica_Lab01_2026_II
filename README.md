# Laboratorio 1: Trayectorias, Entradas y Salidas Digitales - Robótica Industrial

**Universidad Nacional de Colombia**

* **Integrante 1:** [Escribe aquí tu Nombre Completo] - [Documento/Usuario GitHub]
* **Integrante 2:** [Escribe aquí el Nombre de tu Compañero] - [Documento/Usuario GitHub]

---

## 1. Descripción del Proyecto
Este proyecto consiste en la implementación y automatización de una celda de trabajo industrial para la decoración automática de un pastel utilizando un robot **ABB IRB 140** e integración con una **banda transportadora**.

Para el desarrollo de la práctica, se diseñó y modeló en CAD una **herramienta personalizada (porta-marcador)** y una representación física del pastel. Tras importar los modelos a **RobotStudio**, se realizó la calibración y definición del TCP (`tooldata`) junto con la creación de los sistemas de coordenadas de trabajo (`wobjdata`) para garantizar la reutilización y precisión de las trayectorias.

La rutina programada en **RAPID** ejecuta el trazado continuo de los **nombres de los integrantes** del equipo y un diseño creativo personalizado (**Pac-Man**). El ciclo automático sincroniza el transporte del pastel mediante la banda, la detención en el espacio de trabajo, la ejecución de la decoración, el retorno seguro del manipulador a la posición **HOME** y la salida del producto final mediante la gestión de entradas y salidas digitales (`DI`/`DO`).

---

## 2. Plano de Planta
*Ubicación del robot ABB IRB 140, la banda transportadora, las superficies de trabajo y zonas de interacción.*

![Plano de Planta](plano_planta.png)

---

## 3. Diagrama de Flujo
*Secuencia de operación lógica para el ciclo automático (DI_01), pose de mantenimiento (DI_02) y rutina Farmear Aura (DI_03).*

![Diagrama de Flujo](diagrama_flujo.png)

---

## 4. Diseño y Calibración de la Herramienta

### 4.1. Diseño CAD de la Herramienta y Pastel
* **Herramienta:** Diseñada para alojar un marcador/plumón acoplado al *flange* del robot ABB IRB 140.
* **Archivos 3D:** Los modelos en formato `.stl` / `.sat` se encuentran cargados en la raíz de este repositorio.

### 4.2. Comparación de Calibración TCP (`tooldata`)

| Parámetro | Calibración Robot Real | Calibración RobotStudio | Diferencia / Error |
| :--- | :--- | :--- | :--- |
| **X (mm)** | [Escribe valor real] | [Escribe valor virtual] | [Diferencia] |
| **Y (mm)** | [Escribe valor real] | [Escribe valor virtual] | [Diferencia] |
| **Z (mm)** | [Escribe valor real] | [Escribe valor virtual] | [Diferencia] |

* **Análisis de diferencias:** [Escribe aquí brevemente por qué variaron los datos, por ejemplo: imprecisiones al tocar las puntas de calibración, tolerancias de impresión 3D o juego mecánico del marcador].

---

## 5. Descripción del Código RAPID

### 5.1. Instrucciones y Funciones Utilizadas
* **`MoveJ` / `MoveL`:** Control de movimiento articular para aproximación y lineal para trazado continuo sobre el pastel.
* **`Set` / `Reset`:** Activación/desactivación de salidas digitales (`DO_01`, `DO_02`, `DO_03`) y sentidos de la banda (`FWD_Conveyor`, `BWD_Conveyor`).
* **`WaitTime`:** Pausas de temporización para el control de la banda transportadora.
* **`IF` / `FOR` / `WHILE`:** Lógica de enclavamiento, ciclos de ejecución y conmutación entre modos principales.
* **`wobjdata`:** Objeto de trabajo definido en cuadrante $x(+), y(+)$ y reutilizado en $x(+), y(-)$.

### 5.2. Código Fuente
El código RAPID fuente completo se encuentra disponible en el archivo [`codigo_principal.mod`](./codigo_principal.mod).

---

## 6. Evidencias de Entradas y Salidas Digitales

* **`DI_01` & `DO_01` (Ciclo de Decoración):** 
  ![Evidencia DI_01](evidencia_di01.png)
* **`DI_02` & `DO_02` (Pose de Mantenimiento):** 
  ![Evidencia DI_02](evidencia_di02.png)
* **`DI_03` & `DO_03` (Rutina Farmear Aura):** 
  ![Evidencia DI_03](evidencia_di03.png)
* **Banda Transportadora (`FWD_Conveyor` / `BWD_Conveyor`):** 
  ![Evidencia Banda](evidencia_banda.png)

---

## 7. Vídeo de la Práctica
El vídeo publicado en el canal oficial **LabSIR** de la Universidad Nacional de Colombia se puede visualizar en el siguiente enlace:

👉 [**Ver Vídeo del Laboratorio No. 01 en YouTube**](PEGA_AQUÍ_EL_LINK_QUE_TE_DÉ_EL_MONITOR)
