# Nequi_Prueba_Tecnica_Cientifico_Datos
Se presenta prueba Técnica para la  solución de identificar transacciones que evidencian un  comportamiento de Mala Practica Transaccional, empleando un producto de datos.

### Elaborado por: Valentina Gómez Garzón

## Contenido
Este proyecto contiene tres archivos:

  * Readme con indicaciones importantes.
  * Jupyter notebook donde está detallado todo el proceso.
  * PDF con explicación de arquitectura de la solución.

## Objetivo
Idear una solución para identificar transacciones que evidencian un comportamiento de Mala Practica Transaccional, empleando un producto de datos.
Una Mala Practica Transaccional es un comportamiento donde se evidencia un uso de los canales mal intencionado, para la prueba técnica nos centraremos en la práctica de Fraccionamiento Transaccional, esta mala práctica consiste en fraccionar una transacción en un número mayor de transacciones con menor monto que agrupadas suman el valor de la transacción original.

#### Características: 
Estas transacciones se caracterizan por estar en una misma ventana de tiempo que suele ser 24 horas y tienen como origen o destino la misma cuenta o cliente.

## Metodología
Por problemas de capacidad de memoria se escoge solo un dataset para el análisis y entrenamiento de modelos.

###### Lenguaje de programación: Python.

Se usó Jupyter Notebook  que es una aplicación web que sirve a modo de puente constante entre el código y los textos explicativos. El programa se ejecuta desde la aplicación web cliente que funciona en cualquier navegador estándar. 

1. Librerías.
2. Lectura de los datos.
3. Variables.
4. Análisis descriptivo variables iniciales.
5. Agrupamiento y creación de variables.
6. Análisis Variable respuesta: Posible Fraude.
7. Transformación de datos.
8. Modelos.
            8.1 Random Forest
            8.2 Random Forest con class_weight='balanced_subsample
            8.3 Regresión logística con seleccion de variables
            8.4 Regresión logística con Class weighting (solucionaria el desbalanceo)
            8.5 Xgboost
            8.6 Decision Tree
            8.7 Gradient Bosting
            8.8 Ada Boost
9. Análisis de errores.
10. Selección de mejor modelo.
11. Arquitectura del proceso.
12. Trabajos futuros.


## Métricas
Existen varias métricas para escoger un modelo, pero por su relevancia y análisis para este ejercicio se escogieron dos:
#####  F1
Accuracy se puede usar cuando las clase están balanceadeas, mientras que F1 score es una métrica mejor cuando hay clases desbalanceadas. Por esto, para este caso como tenemos un dataset desbalanceado, se usó esta métrica.


###### Curva ROC Y área AUC: 
Esta es una de las métricas más importantes utilizadas para medir el rendimiento de un modelo de clasificación. El área bajo la curva ROC se conoce como AUC, cuanto mayor es el valor de AUC mejor es el modelo. Cuanto más lejos esté la curva ROC de la línea diagonal (y = x), mejor será el modelo.
Así es como ROC-AUC nos permite evaluar el rendimiento de nuestro modelo, y nos proporciona un medio para seleccionar un modelo.

## Criterios de selección
Se realizaron varios modelos, unos con selección de variables, ajustes de hiperparámetros y solucionando el desbalanceo presente para este ejercio. Comparando cada una de las dos métricas seleccionadas (F1 y curva ROC-AUC), el modelo seleccionado fue:

En el jupyter se presenta los ranking de los modelos entrenados.

Aunque otros modelos dieron métricas mucho mejores, debemos evitar el overfiting, conocido como el sobre ajuste, donde funciona excelente los datos como se entrenó el modelo pero a la hora de seleccionar otros datos, tiene un pésimo rendimiento. Se sacrifica rendimiento, pero se garantiza no caer en overfiting.

Recordamos que la forma de evitar el overfiting y logrando cada vez un mejor modelo es haciendo ajuste de hiperparámetros, selección de variables y solucionando los desbalanceos de clases, realizando pruebas de validación cruzada.

## Arquitectura de la solución

La arquitectura del proceso  de entrenamiento:
    
    1. Captura de información directamente desde aws usando spark.
    2. Agrupamientos para seleccionar e idenficar los fraccionamientos transaccionales
    3. Realizar depuración, valores atípicos y molimiento de datos.
    4. división en datos de entrenamiento y test 70-20.
    5. Quitamos el desbalanceo de dos formas diferentes: con SMOTEC Y oversampling
    6. Gracias a el uso de pipline se tiene un proceso automatizado que consta de:
        * Preprocesamiento donde se hace la escalada de variables
        * Preprocesamiento para las variables categóricas para hacer el OneHotEncoder
        * Entrenamiento del modelo respectivo.
        * Ajuste de hiperparámetros.
    7. Con la muestra de testeo se sacan las métricas mas importantes para saber el rendimiento del modelo.
    8. Se selecciona el mejor modelo.
    9. Paso a producción.
   
La arquitectura del proceso ya en producción quedaría de la siguiente forma:

    1. Recolección de información en tiempo real, un día vecido.
    2. Realizar la predicción con el modelo seleccionado anteriormente.
    3. Entregar resultados.
    4. Se ingesta de nuevo la data y se hace un nuevo la arquitectura de entrenamiento para ir ajustando de nuevo el modelo.
    
Con esta arquitectura tendríamos un proceso que se podría ejecutar la predicción al cabo de 24 horas, donde se podría ir reentrenando, asegurando la captura de los fraccionamientos transaccionales.

## Trabajos Futuros
* Para poder cargar todos los datos, se sugiera hacer la carga y el paso de modelo a spark. Spark ha establecido récords mundiales en cuanto a velocidades de procesamiento se refiere. Se pasaría toda la arquitectura y procesamiento a dicha herramienta.

* Trabajo próximo sería pensar un un forecast pero utilizando ya series de tiempo para pronosticar en promedio cuantos fraccionamientos transaccionales se podrían tener en un día sin haber corrido y los corresponsales mas prospensos a dicho fraude.


