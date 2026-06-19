# PROYECTO_23310320_23310378_6F
Alumnos:
Alex Durán de Huerta Jiménez - 23310378
Luis Arturo Curiel Rodriguez - 23310320

# Sistema de Inspección y Clasificación Automatizada de plátanos basado en Visión Artificial mediante la Arquitectura YOLO

## 1. Descripción del Proyecto

Este proyecto implementa un sistema de visión artificial basado en la arquitectura de red neuronal convolucional YOLO (You Only Look Once), optimizado para la detección, localización y clasificación en tiempo real de plátanos (*Musa paradisiaca*). 

El modelo procesa flujos de video o imágenes estáticas para generar coordenadas de delimitación (*bounding boxes*) y asignar etiquetas de clase junto con su respectivo índice de confianza probabilístico. El objetivo principal es demostrar la viabilidad de la transferencia de conocimiento (*Transfer Learning*) utilizando modelos de detección de objetos de un solo paso para su posterior despliegue en entornos comerciales, logísticos y agrícolas.

## 2. Instrucciones de Instalación y Uso

Requisitos Previos:
  * Python 3.8 o superior
  * Entorno de ejecución con soporte para CUDA (opcional, para aceleración por GPU)

Configuración del Entorno:
1. Clonar el repositorio:
   git clone [https://github.com/tu_usuario/tu_repositorio.git](https://github.com/tu_usuario/tu_repositorio.git)
   cd tu_repositorio

2. Crear e inicializar un entorno virtual:
   python -m venv env
   # En Windows:
   env\Scripts\activate
   # En Linux/macOS:
   source env/bin/activate
   
3. Instalar las dependencias del proyecto:
   pip install -r requirements.txt
   
4. Ejecución del Modelo
Para ejecutar la inferencia en imágenes o videos de prueba, ejecute el script principal pasando como argumento la ruta del archivo origen:
   python inferencia.py --source muestras/test_platano.jpg

## 3. Caso de Estudio: Integración en Entornos Reales e Industriales

La versatilidad de este modelo YOLO entrenado para la detección de plátanos permite su abstracción e integración en dos casos de uso críticos dentro de la cadena de suministro alimentaria: la automatización del punto de venta (Retail) y la cosecha mecanizada (Sector Agrícola).

### Escenario A: Terminal de Autocobro (Cajero Inteligente de Supermercado)

* **Problema a Resolver:** Reducir la fricción en el proceso de checkout y mitigar el fraude o errores operativos por parte del usuario en las básculas tradicionales de autoservicio, donde la selección manual del producto (como el ingreso erróneo del código PLU) genera pérdidas económicas y demoras en las filas de atención.
* **Arquitectura de Hardware Propuesta:**
  * **Sistema de Adquisición de Imagen:** Cámara web de alta resolución (1080p) con lente gran angular integrada en la estructura superior del cajero, orientada verticalmente hacia el plato de la báscula.
  * **Procesamiento de Datos:** Computadora embebida de factor de forma pequeño (SFF) con aceleración de hardware para inteligencia artificial en el borde (Edge Computing), basada en módulos de procesamiento neuronal optimizado.
  * **Interfaz de Control:** Software del punto de venta (POS) que interactúa directamente con la interfaz del usuario y los sistemas de pago.
* **Flujo de Funcionamiento:**
  1. El cliente coloca un racimo de plátanos sobre la báscula del cajero automático.
  2. La cámara captura el flujo de video del área de pesaje de manera continua.
  3. El script de inferencia procesa el cuadro en un tiempo de respuesta inferior a 15 milisegundos. Si el modelo YOLO detecta la clase `platano` con una confianza superior o igual al 85%, congela la inferencia.
  4. El script envía un payload en formato JSON al software del punto de venta con la etiqueta del producto validado. El sistema del cajero cruza este dato con el peso medido por la báscula y actualiza automáticamente la pantalla de cobro con el producto correcto sin requerir interacción manual del usuario.

### Escenario B: Cosecha Automatizada y Clasificación en Campo (Agrotecnología)

* **Problema a Resolver:** Optimizar los tiempos de recolección en campo, mitigar la escasez de mano de obra y estandarizar el control de calidad en tiempo real directamente en la maquinaria agrícola destinada a la recolección de múltiples frutos simultáneos.
* **Arquitectura de Hardware Propuesta:**
  * **Sistema de Adquisición de Imagen:** Cámaras de grado industrial con certificación IP67 (resistentes a polvo y agua), equipadas con sensores de obturación global (*global shutter*) para evitar el desenfoque por movimiento, montadas sobre el brazo recolector o tolva del vehículo móvil.
  * **Procesamiento de Datos:** Computadora industrial robustecida contra vibraciones mecánicas y altas temperaturas, equipada con una unidad de procesamiento gráfico (GPU) de arquitectura dedicada.
  * **Actuadores y Control:** Un Controlador Lógico Programable (PLC) industrial comunicado mediante protocolos de bus de campo (Modbus TCP o CAN bus), acoplado a un sistema de actuadores neumáticos y electroválvulas en la banda transportadora del vehículo recolector.
* **Flujo de Funcionamiento:**
  1. Durante la marcha del vehículo de recolección por las hileras de cultivo, las cámaras adquieren video en tiempo real de los frutos que ingresan a la tolva de clasificación.
  2. El modelo YOLO realiza la segmentación y clasificación de los diferentes frutos en un entorno de fondo complejo compuesto por hojas, ramas y suelo.
  3. Al identificar espacialmente los plátanos mediante las coordenadas de píxeles $(x, y)$ en la matriz de la imagen, el algoritmo calcula su posición relativa en la banda.
  4. Si el fruto cumple con los criterios de detección y el umbral mínimo de confianza, el script envía una señal digital de activación al PLC. Este activa un pistón de rechazo o un deflector neumático en la banda segmentadora para desviar mecánicamente los plátanos hacia su contenedor específico, separándolos automáticamente del resto de los frutos recolectados de manera simultánea.

## 4. Evidencias de Funcionamiento

Las imágenes resultantes de la validación del modelo se encuentran almacenadas en el directorio `evidencias/`. A continuación se despliega una muestra de las detecciones del modelo tras el proceso de entrenamiento:

![Muestra de Detección 1](evidencias/deteccion_platano_1.jpg)
*Figura 1: Inferencia en entorno controlado con cajas de delimitación y porcentaje de precisión sobre la clase detectada.*
