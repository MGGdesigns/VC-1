# Visión por Computador: Práctica 1

## Introducción
Esta primera práctica tiene como objetivo, famialiarizarse con las herramientas de desarrollo [Anaconda](https://www.anaconda.com/), el manjedor de paquetes que se empleará para realizar las instalaciones de los paquetes necesarios durante el deasrrollo de las sesiones prácticas de la asignatura. Además de comprender y saber hacer operaciones básicas de tratamiento de imágenes, como acceder a los valores asociados a un píxel en
concreto, modificarlos, abrir una imágen de disco, acceder a los fotogramas de un vídeo o captura cámara, entre otros. Para ello, se proponen las tareas desglosadas más abajo.

## Tareas

### Tarea 1: Creación de una imagen con textura de ajedrez 800x800

Como primera tarea se propone la creación de una imagen con textura de ajedrez que tenga un tamaño de 800x800 píxeles (px), [ejemplo](https://www.lacasadelajedrez.com/articulos/tablero-de-ajedrez-de-competicion-plastico-45-cm/691/). Para ello, en primera instancia se crea una imágen en escala de grises, inicializada a negro, en la que se pintarán los cuadrados blancos del tablero. El procedimiento seguido constó en definir una variabele *cuadrado* que actuará como el tamaño tanto en alto como en ancho (100px) de los cuadrados del tablero que se utilizará para seleccionar las filas y columnas a pintar. Una vez aclarado esto, se debe abordar el recorrido de la imagen, se decidió iterar sobre la misma como si fuese un tablero real, es decir, recorrer las ocho filas y ocho columnas. Lo que nos deja con coordenadas o posiciones *(i,j)*, tales como (0,0), (0,1), esto permite detectar los cuadrados blancos al realizar la operación *(i+j) % 2 *. Donde, si el resto es cero, se ha detectado una posición en la que debe haber un cuadrado blanco. Ahora, queda simplemente tratar el tema de cómo pintar
el cuadrado de blanco. Esto, se realiza con la ayuda de la variable *cuadrado*, que junto a las coordenadas *(i,j)* permiten la selección de las filas y coulumnas a pintar de la imagen como se muestra en la operación siguiente: *chess_img[i*cuadrado:(i+1)*cuadrado, j*cuadrado:(j+1)*cuadrado]=255*. Lo que cambia los valores de las filas y columnas que van desde *i*cuadrado* y *j*cuadrado* hasta *(i+1)*cuadrado* y *(j+1)*cuadrado*, a 255 el valor asociado con blanco.

Ejemplo primera iteración: ```chess_img[0*100:(0+1)*100, 0*100:(0+1)*100]=255``` Lo que estaría pintando desde la fila 0 hasta la 99 y desde la columna 0 a la 99 de blanco.

### Tarea 2: Crear una imagen estilo Mondrian

Aquí se propone crear una imagen a color como aquellas [obras](https://www.reprodart.com/a/mondrian-piet/composicion-con-rojo-amarillo-azul-y-negro.html) de Piet Mondrain. Para hacerlo, se comienza con una imagen a color inicializada a negro de 200x300 píxeles, que mediante selección de submatrices se irá pintando para que se asemeje a las obras de Mondrain. Lo cual, se logra seleccionando el rango de filas y columnas a pintar y en este caso el canal específico, ya que, se cuenta con tres canales.

Ejemplo: ```color_img[0:125,0:150,0] = 255``` Pinta desde la fila cero hasta la 124 y desde la columna 0 hasta la 149 de rojo [R,G,B].

### Tarea 3: Resolver una de las tareas anteriores con las funciones de dibujo de OpenCV

Se ha decido volver a resolver la Tarea 2, pero esta vez con las funciones de dibujo de OpenCV, como se vuelve a utilizar la imagen generada anteriormente, es necesario realizar un limpiado de imagen. Para ello, se pintan los tres canales al mismo valor, 255, de la forma *color_img[:,:,0] = 255*. Obteniendo así, una imagen completamente blanca. Una vez hecho esto, se emplea el comando *cv2.rectangle(img, p1, p2, color, thickness)*, que recibe como parámetros una imagen, dos puntos inicial y final, el color en formato escalar y un entero que representa el grosor del borde. También se usa el comando *cv2.line(img, p1, p2, color, thickness)*, que recibe los mismos parámetros que el anterior. Como sus nombres indican, estos se usan para dibujar rectángulos y líneas respectivamente. Recalcar que primero se dibujan los rectángulos y después las líneas, esto se hace así para que los rectángulos queden bien cuadrados con las líneas.

### Tarea 4: Modificar de forma libre los valores del plano de una imagen

Se entrega un código de ejemplo en el que se juega con los valores de los canales RGB y se propone, cambiar los valores para uno de los planos. Recalcar que cómo se está capturando la cámara web con la librería *cv2*, los canales se invierten, ya que, de esta forma interpreta las imágenes dicha librería (RGB --> BGR). Se decidió cambiar los valores del plano rojo (2), concretamente, se decidió invertir sus valores. Para lograrlo, una vez obtenida la imagen con únicamente el plano rojo *r=frame[:,:,2]*, se realiza la operación *r=255-r*, invirtiendo así los colores del plano. Esta operación se deduce, de que al invertir un plano en escala de grises, el valor 255 pasa a ser negro y el 0 se convierte en blanco. Por tanto si, se quiere invertir *x* cuyo valor es 0, se haría de la forma *invX=255-x*. Lo cual da como resultado 255, pasando así el píxel que anteriormente era negro a blanco.



