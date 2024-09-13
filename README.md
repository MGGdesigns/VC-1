# Visión por Computador: Práctica 1

## Introducción
Esta primera práctica tiene como objetivo, famialiarizarse con las herramientas de desarrollo [Anaconda](https://www.anaconda.com/), el manjedor de paquetes que se empleará para realizar las instalaciones de los paquetes necesarios durante el deasrrollo de las sesiones prácticas de la asignatura. Además de comprender y saber hacer operaciones básicas de tratamiento de imágenes, como acceder a los valores de un píxel en concreto, crear una imagen, dibujar primitivas gráficas sobre una imagen, abrir una imagen de disco, acceder a los fotogramas de un vídeo, entre otros. Para esto, se dispone de una serie de tareas, que se comentan más abajo.

## Tareas

### Tarea 1: Crear una imagen 800x800 píxeles, con la textura de un tablero de ajedrez

Para llevar a cabo esta tarea, se establece una imagen del tamaño pedido en escala de grises inicializada a 0, es decir en negro. Seguidamente se inicializa la variable *cuadrado* a 100 píxeles, esta representará el tamaño de los cuadrados del tablero y será de ayuda a la hora de pintar los cuadrados blancos. La filosofía escogida para recorrer la imagen se ha centrado en tratarla como si de un tablero real de ajedrez se estuviese hablando. Es decir, mediante sus 8 filas y 8 columnas, lo cual nos deja con las coordenadas o posiciones *i,j*, que se usaran para determinar si el cuadrado debiese ser coloreado o no. Esto se determina al realizar el módulo 2 de la suma de ambas coordenadas, como se muestra en la operación:

                                                                    ```(i+j) % 2```

En el caso que su resultado sea 0, estamos ante uno de los varios cuadrados del tablero que deberían ser pintados de blanco. Para esto, se retoma la variable mencionada anteriormente, que junto a las coordenadas *i,j*, será de ayuda a la hora de seleccionar las filas y columnas a pintar de la imagen. Dado que *i,j* sí recorren la imagen en su totalidad pero no muestran los índices necesarios como para ir pintando de 100 en 100 filas y columnas, es ahí donde entra en juego la operación siguiente:

                                            ```chess_img[i*cuadrado:(i+1)*cuadrado, j*cuadrado:(j+1)*cuadrado]=255```

Entiendo *i,j* como alto y ancho de la imagen, respectivamente. Con la operación anterior se van seleccionando las filas desde i·100 hasta (i+1)·100 y las columnas j·100 hasta (j+1)·100. Por lo que efectivamente, se pintan cuadrados de 100x100 píxeles.

### Tarea 2:

### Tarea 3:

### Tarea 4:

### Tarea 5:

### Tarea 6:
