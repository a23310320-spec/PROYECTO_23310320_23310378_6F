# PROYECTO_23310320_23310378_6F

# Sistema de Inspección y Clasificación Automatizada de Plátanos en el Sector Agrícola mediante Visión Artificial

**Integrantes:**
* Alex Durán de Huerta Jiménez - Código de estudiante: 23310378
* Luis Arturo Curiel Rodriguez - Código de estudiante: 23310320

## Índice
1. [Descripción del Proyecto](#descripción-del-proyecto)
2. [Caso de Estudio: Optimización en el Campo Agrícola](#caso-de-estudio-optimización-en-el-campo-agrícola)
   * [2.1 Planteamiento del Problema a Resolver](#21-planteamiento-del-problema-a-resolver)
   * [2.2 Descripción del Hardware Propuesto](#22-descripción-del-hardware-propuesto)
   * [2.3 Flujo de Funcionamiento del Sistema](#23-flujo-de-funcionamiento-del-sistema)
3. [Requisitos del Sistema](#requisitos-del-sistema)
4. [Instrucciones para Correr el Código](#instrucciones-para-correr-el-código)

---

## Descripción del Proyecto
Este proyecto implementa un sistema inteligente de visión artificial basado en la arquitectura YOLOv8 para la automatización del control de calidad del plátano directamente en las fases de postcosecha y empaque. A diferencia de los sistemas de clasificación convencionales que solo detectan la presencia del fruto, este modelo realiza un análisis multiclase avanzado para inspeccionar la superficie, estructura y madurez de cada unidad, integrando un módulo de conteo estadístico para auditar los defectos en tiempo real.

---

## Caso de Estudio: Optimización en el Campo Agrícola

### 2.1 Planteamiento del Problema a Resolver
En el sector agrícola, la clasificación del plátano posterior a la corta se realiza mayoritariamente de forma manual por operadores humanos. Este método tradicional presenta serias desventajas:
* **Alta variabilidad y subjetividad:** Fatiga visual y criterios inconsistentes entre operarios al evaluar defectos.
* **Daños mecánicos inadvertidos:** Raspones leves o microfisuras en el tallo pasan desapercibidos, pudriendo lotes enteros durante el transporte marítimo.
* **Falta de métricas en tiempo real:** Imposibilidad de cuantificar con precisión matemática cuántos defectos presenta cada pieza para asignarle una categoría de calidad exacta (Premium, Primera, Segunda o Desecho), lo que genera pérdidas por penalizaciones comerciales o devoluciones.

### 2.2 Descripción del Hardware Propuesto
Para desplegar el modelo entrenado en un entorno real de empacadora agrícola, se plantea la integración de la siguiente arquitectura de hardware:

* **Sistema de Captura de Imagen (Cámaras):** Se proponen 3 cámaras industriales de visión artificial de grado IP67 (resistentes a humedad y polvo del campo), equipadas con sensores Sony IMX y lentes de longitud focal fija para evitar distorsiones. Están dispuestas en un túnel de inspección a 120° una de otra para obtener una reconstrucción visual de 360° del plátano.
* **Unidad de Procesamiento Analítico:** Computadora embebida industrial dotada de una GPU **NVIDIA Jetson Orion Nano** (de 8 GB de RAM). Este hardware permite ejecutar la red neuronal YOLOv8 de manera local (*Edge Computing*) con una latencia mínima, procesando el flujo de video a más de 30 fotogramas por segundo sin requerir conexión a internet.
* **Iluminación Controlada:** Difusores de luz LED tipo *Dome Light* de alta intensidad para eliminar sombras complejas, destellos por la humedad natural de la cáscara y garantizar que el color de la madurez se evalúe bajo un estándar constante.
* **Actuadores y Maquinaria Automática:** Un controlador lógico programable (**PLC**) conectado a la Jetson mediante el protocolo Modbus TCP. El PLC recibe las señales de clasificación y activa pistones neumáticos eyectores o compuertas mecánicas al final de la banda transportadora para desviar físicamente el producto a su respectivo contenedor de calidad.

### 2.3 Flujo de Funcionamiento del Sistema
El proceso de inspección automatizada se rige bajo la siguiente secuencia lógica:

1. **Transporte y Detección de Proximidad:** Los plátanos avanzan colgados de un polipasto o sobre una banda transportadora de velocidad regulada. Un sensor fotoeléctrico detecta la entrada del fruto al túnel de iluminación.
2. **Captura y Preprocesamiento:** Las cámaras capturan imágenes simultáneas de alta resolución al recibir el disparo del sensor. El frame se escala a $640 \times 640$ píxeles para coincidir con la entrada nativa del modelo.
3. **Inferencia de la Red Neuronal (YOLOv8):** El modelo evalúa los vectores visuales e identifica las cajas delimitadoras de las siguientes clases:
   * *Madurez:* Clasifica el fondo cromático en **Dulce** (apto para anaquel inmediato) o **No Dulce** (verde/maduración en transporte).
   * *Defectos de Superficie:* Localiza y segmenta zonas con **Raspado** o **Manchas**.
   * *Defectos Críticos:* Identifica áreas con **Daño en el tallo**, **Manchas de daño** o estructura **Abierta**.
4. **Conteo Estadístico y Clasificación:** El script analiza el arreglo de salida. Si la clase *Abierto* o *Daño en el tallo* es mayor o igual a 1, se etiqueta instantáneamente como **Desecho**. Si los contadores de *Raspado* o *Manchas* superan un umbral tolerable establecido por la norma agrícola, se clasifica como **Segunda**. Si el fruto está limpio y en estado óptimo, se etiqueta como **Premium**.
5. **Acción Física:** La Jetson envía la palabra de estado al PLC indicando la categoría asignada. El PLC calcula el tiempo de retraso basado en la velocidad de la banda y activa el pistón neumático correspondiente, depositando de forma automatizada el plátano en su contenedor final de destino.

---

## Requisitos del Sistema
El archivo `requirements.txt` contiene las dependencias esenciales para desplegar la aplicación:
```
ultralytics>=8.0.0
roboflow>=1.0.0
opencv-python>=4.6.0
numpy>=1.21.0
```
## Instrucciones para Correr el Código

### 1. Preparación del Entorno local
Clone el repositorio desde la terminal e instale las librerías del sistema:
```
git clone [https://github.com/tu-usuario/tu-repositorio.git](https://github.com/tu-usuario/tu-repositorio.git)
cd tu-repositorio
pip install -r requirements.txt
```
### 2. Proceso de Entrenamiento
Para ejecutar el script de entrenamiento y reentrenar la red neuronal con el conjunto de datos de Roboflow, ejecute:
```
python entrenar.py
```
### 3. Ejecución del Sistema de Inferencia y Conteo en Campo
Para activar la cámara de inspección en tiempo real, procesar el flujo de video y desplegar el contador de calidad en la terminal de control, utilice el siguiente comando:


## 2. Instrucciones de Instalación y Uso

### Requisitos previos
* Python 3.8 o superior
* Cuenta gratuita en [Roboflow](https://roboflow.com) (para descargar el dataset del proyecto `comvis-banana/comvis-banana-2`)
* Entorno con soporte CUDA (opcional — acelera el entrenamiento; el proyecto también corre en CPU, solo que más lento)

### Configuración del entorno

1. **Clonar el repositorio**
```bash
   git clone https://github.com/tu_usuario/tu_repositorio.git
   cd tu_repositorio
```

2. **Crear e inicializar un entorno virtual**
```bash
   python -m venv env
   # En Windows:
   env\Scripts\activate
   # En Linux/macOS:
   source env/bin/activate
```

3. **Instalar las dependencias**
```bash
   pip install -r requirements.txt
```
   Esto instala `roboflow` (descarga del dataset) y `ultralytics` (entrenamiento e inferencia con YOLOv8).

4. **Configurar tu clave de API de Roboflow**

   Por seguridad, la API key **no** debe quedar escrita en el código que subes a GitHub. Defínela como variable de entorno:
```bash
   # Linux/macOS
   export ROBOFLOW_API_KEY="tu_api_key_aqui"
   # Windows (PowerShell)
   $env:ROBOFLOW_API_KEY="tu_api_key_aqui"
```
   Tu clave personal se obtiene gratis en: https://app.roboflow.com/settings/api

5. **Descargar el dataset y entrenar el modelo**
```python
   from roboflow import Roboflow
   import os

   rf = Roboflow(api_key=os.environ["ROBOFLOW_API_KEY"])
   project = rf.workspace("comvis-banana").project("comvis-banana-2")
   version = project.version(1)
   dataset = version.download("yolov8")   # ver nota abajo

   import ultralytics
   ultralytics.checks()   # verifica si hay GPU disponible o se usará CPU

   from ultralytics import YOLO
   model = YOLO('yolov8n.pt')
   results = model.train(
       data=f"{dataset.location}/data.yaml",
       epochs=30,
       imgsz=640,
       plots=True
   )
```

6. **Probar el modelo con una imagen de ejemplo**
```python
   !wget -nc <URL_de_una_imagen_de_platanos> -O imagen_prueba.jpg

   from PIL import Image
   results = model('imagen_prueba.jpg')
   for r in results:
       im_array = r.plot()
       im = Image.fromarray(im_array[..., ::-1])
       display(im)
```
   Las imágenes resultantes (con bounding boxes y nivel de maduración) se guardan automáticamente en `runs/detect/`.
