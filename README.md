# Amazon SageMaker Autopilot - Clasificación Multiclase
### Versión 1.0.0 

---

© 2020 Amazon Web Services, Inc. y sus filiales. Todos los derechos reservados. Esta obra no puede ser reproducida o redistribuida, en su totalidad o en parte, sin el permiso previo por escrito de Amazon Web Services, Inc. Se prohíbe la copia comercial, préstamo o venta.

¿Errores o correcciones? Envíenos un correo electrónico a [mboaglio@amazon.com](mailto:mboaglio@amazon.com).

---

## Introducción
Los archivos del presente repositorio son parte de una demostración de Amazon SageMaker Autopilot realizando una clasificación multiclase. Durante la misma se explican los conceptos de experiment, trial, objective metric, feature engineering e inference pipeline y se repasa el proceso de modelado predictivo de un data scientist (con y sin el beneficio que le aporta SageMaker Autopilot).

Se utiliza el siguiente dataset:
### Human Activity Tracking w/ Smartphone Dataset
#### Kaggle Simplified Version: https://www.kaggle.com/mboaglio/simplifiedhuarus

Que es una simplificación del siguiente dataset:
### UCI Maching Learning Repository - Dataset Original
### Abstract: Recordings of subjects performing activities while carrying inertial sensors.
#### Link: https://archive.ics.uci.edu/ml/datasets/human+activity+recognition+using+smartphones

## Detalles
El dataset contiene grabaciones de personas que realizaron actividades mientras llevaban sensores de inercia (o acelerometros).
Cada registro tiene diferentes mediciones (561 en total) asociadas a una etiqueta que describe la actividad: WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING y LAYING

Para cada registro se proporciona:
1. Aceleración triaxial del acelerómetro (aceleración total) y la aceleración corporal estimada.
2. Velocidad angular triaxial desde el giroscopio.
3. Un vector de características 561 con variables de dominio de tiempo y frecuencia.
4. Su etiqueta de actividad (sobre la cual vamos a trabajar en la clasificación).

## ¿Como sería el proceso?
### Paso 1 
Utilizando el archivo #01 bajamos el dataset, hacemos un split y obtenemos los siguientes datos para iniciar el Autopilot en Experiments:

1. Experiment Name
[Por ejemplo] Autopilot-HAC-DEMO

2. S3 location of input data: 
s3://sagemaker-us-east-2-XXXXXXXXXXXX/sagemaker/DEMO-autopilot-had-dataset/input/had-autopilot-train.csv

3. Target attribute name:
activity

4. S3 location for output data: 
s3://sagemaker-us-east-2-XXXXXXXXXXXX/sagemaker/DEMO-autopilot-had-dataset/output

### Paso 2
Los archivos #02 y #03 son generados automaticamente por Amazon SageMaker. En este repositorio solo se incluyen a modo de ejemplo ya que Sagemaker los ejecuta como parte del experimento. Si los abrimos vamos a poder observar que fueron ejecutados, que contienen información sobre los modelos elegidos y los inference pipelines construidos. Esto se debe a que fueron ejecutados a posteriori y en forma separada al experimento.

### Paso 3
Una vez finalizado el Experimento, tenemos que crear un endpoint de inferencia. Para ello elegimos el mejor Trial del experimento y con el boton derecho sobre el mismo desplegamos el menu que tiene la opción "Create Endpoint". En la psiguiente pantalla, solo completamos el nombre y dejamos el resto con los datos default.

### Paso 4
Una vez creado el endpoint, modificamos el archivo #04 con el nombre eleguido arriba y lo ejecutamos. El archivo #04 recorrerá el dataset de test y realizará la inferencia sobre cada registro. Al finalizar calculará el accuracy y la matriz de confusión del modelo elegido.


## Archivos/notebooks incluidos

### 01_HumanActivityDataset-Split.ipynb
Este notebook baja el dataset de Github y lo divide en un 95% para entrenamiento y un 5% para la inferencia final. Esta división es adicional a la de validacion y entrenamiento, que va a ser realizada por Amazon SageMaker Autopilot en forma automática cuando inicie su análisis.
El dataset de entrenamiento es subido a un bucket Amazon S3, y el 5% restante será utilizado por el notebook #04. 

### 02_SageMakerAutopilotDataExplorationNotebook.ipynb
Notebook creado por Amazon SageMaker Autopilot para explorar el dataset.

### 03_SageMakerAutopilotCandidateDefinitionNotebook.ipynb	
Notebook creado por Amazon SageMaker Autopilot una vez que analizó los datos y eligió los modelos candidatos a ser optimizados. El proceso incluye el armado de un pipeline de inferencia que resuelve temas como la imputacion de datos faltantes, o el OneHot encoding de las variables categoricas. También incluye la optimización de hiperparámetros de los modelos candidatos. 

### 04_HumanActivityDataset-Inference.ipynb
Notebook con codigo de inferencia sobre el endpoint que armó Amazon SageMaker Autopilot después de terminar de ejecutar el Notebook #03.
La inferencia se hace en forma directa, con los datos RAW (en CSV), ya que el endpoint incluye los containers de encoding para que el modelo final pueda procesarlos.

### Directorio Autopilot-HAC-DEMO-artifacts
Creado automaticamente por Amazon SageMaker Autopilot y que contiene el codigo de los modelos creados.


## Licencia
This library is licensed under the MIT-0 License. See the LICENSE file.

