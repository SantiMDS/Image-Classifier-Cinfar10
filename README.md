# Creación y Optimización de una Red Neuronal para Cifar-10

## Introducción
En este informe, se detalla el proceso de creación y optimización de una red neuronal convolucional para clasificar 10 tipos diferentes de imágenes dentro del dataset de Cifar-10. Nuestro objetivo principal será que nuestra red consiga acertar en 9/10 imágenes, lo que equivale a un 90% de precisión de la red. Por ello, la métrica seleccionada para medir la precisión sería el accuracy, ya que queremos lograr el mayor número de aciertos sobre el total de imágenes que nuestro modelo intenta predecir.

## Desarrollo de la Red Neuronal
El proceso de creación de la red neuronal comenzó con un primer intento que resultó en un bajo accuracy, inferior al 50%, y contaba con una estructura con un bloque de Convolución2D(64) + Max Pooling que estaba sobreentrenada.

Después de solucionar los problemas de sobreentrenamiento, nuestra CNN base no logró mejorar el 60% de accuracy, por lo que adoptamos la arquitectura básica de la red VGG16 para nuestro modelo.

## Optimización de la Arquitectura

Como habíamos comentado, se adoptó parcialmente la estructura de la VGG16 para las capas convolucionales, que cuenta con 5 bloques de capas convolucionales de dos capas convolucionales por bloque más una capa densa y un clasificador. Pasaremos a detallar las características que se mantuvieron durante el proyecto y qué características fueron modificadas siguiendo un orden lógico.

### Capas Convolucionales

Durante todo el proyecto, se mantuvo la activación ReLU así como los Kernels con un tamaño de 3x3.

No se añadió ningún bloque convolucional, pero sí hubo numerosas modificaciones intrabloque para aumentar la capacidad predictiva del modelo, siempre controlando el sobreentrenamiento que pudiera aparecer.

- **Número de capas dentro del bloque:** Comenzando por 2 capas + Max Pooling, para ir aumentándolas de 1 en 1 hasta llegar a un máximo de 5 capas.
- **Aplicación de BatchNormalization:** Durante todo el desarrollo del proyecto, se comenzó aplicando el BN a la primera capa dentro del bloque y se fue aumentando a las siguientes capas.
- **Dropout:** Al principio, se siguió la aplicación de Dropout después de cada capa de Conv2D, con un Dropout de 0.5 bajando hasta el 0.2; sin embargo, se comprobó que mejoraba mucho más si solo se aplicaba al final del bloque convolucional.

### Capas Densas

Para las capas densas, no utilizamos la estructura de VGG16; optamos por iniciar con la capa de alisado y una capa de 256 neuronas con activación ReLU y la inicialización del Kernel utilizando la distribución uniforme en un rango ajustado para estabilizar el gradiente durante el entrenamiento. En cuanto al clasificador, creamos la capa de 10 neuronas para la clasificación con activación Softmax.

Al final del proyecto, y dado que nuestra red alcanzó un accuracy del 87%, añadimos dos nuevas capas densas con las mismas características, con 256 y 128 neuronas respectivamente.

## Data Augmentation
Debido a que el conjunto de datos era relativamente pequeño, aplicamos la técnica de data augmentation modificando diferentes parámetros.

## Optimización de Parámetros
Durante todo el proyecto, utilizamos el optimizador ADAM, recomendado por el equipo docente y respaldado por la investigación, que lo muestra como uno de los optimizadores que entrenan con mayor rapidez y de los que se obtienen mejores resultados.

Por otro lado, uno de los principales determinantes para la consecución del éxito es el ajuste del Learning Rate. Comenzamos con un Learning Rate de 0.001, el cual fuimos ajustando hasta el 0.00001, donde observamos pérdidas de accuracy. Al final, nos decantamos por aplicar un learning rate descendiente el cual se puede consultar en el código.

En cuanto al Batch size, comenzamos definiéndolo en 516, pero observamos que ajustándolo a 128, obteníamos mejores resultados, ya que mejoraba mucho más rápido el accuracy y reducíamos los tiempos de entrenamiento.

Por último, debemos hablar sobre el EarlyStopping. Durante el entrenamiento del modelo, definimos un número de EPOCHS de 500, pero lo controlamos con el EarlyStopping, el cual estaba definido en 30 al principio para optimizar mejor las capas y los optimizadores, para posteriormente finalizar en un early de 50 para intentar conseguir el 90% de accuracy.

## Resultados y Conclusiones
Finalmente, el modelo logró un accuracy del 90% en el conjunto de datos de prueba después de varias iteraciones y ajustes. Se concluye que la combinación de técnicas de regularización, ajustes de hiperparámetros y la optimización de Data Augmentation fueron fundamentales para mejorar el desempeño del modelo.

## Recomendaciones
Se recomienda continuar explorando otras técnicas de regularización, como Weight Decay, y experimentar con la aplicación de nuevos bloques, aumentando las capas por bloque o utilizando diferentes capas densas para mejorar aún más el desempeño del modelo en futuros proyectos.
