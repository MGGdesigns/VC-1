# Visión por Computador: Práctica 1

## Introducción
Esta primera práctica tiene como objetivo, famialiarizarse con las herramientas de desarrollo [Anaconda](https://www.anaconda.com/), el manjedor de paquetes que se empleará para realizar las instalaciones de los paquetes necesarios durante el deasrrollo de las sesiones prácticas de la asignatura. Además de comprender y saber hacer operaciones básicas de tratamiento de imágenes, como acceder a los valores de un píxel en concreto, crear una imagen, dibujar primitivas gráficas sobre una imagen, abrir una imagen de disco, acceder a los fotogramas de un vídeo, entre otros. Para esto, se dispone de una serie de tareas, que se comentan más abajo.

## Tareas

### Tarea 1: Crear una imagen 800x800 píxeles, con la textura de un tablero de ajedrez

Para llevar a cabo esta tarea, se establece una imagen del tamaño pedido en escala de grises inicializada a 0, es decir en negro. Seguidamente se inicializa la variable *cuadrado* a 100 píxeles, esta representará el tamaño de los cuadrados del tablero y será de ayuda a la hora de pintar los cuadrados blancos. La filosofía escogida para recorrer la imagen se ha centrado en tratarla como si de un tablero real de ajedrez se estuviese hablando. Es decir, mediante sus 8 filas y 8 columnas, lo cual nos deja con las coordenadas o posiciones *i,j*, que se usaran para determinar si el cuadrado debiese ser coloreado o no. Esto se determina al realizar el módulo 2 de la suma de ambas coordenadas, como se muestra en la operación:

```(i+j) % 2```

En el caso que su resultado sea 0, estamos ante uno de los varios cuadrados del tablero que deberían ser pintados de blanco. Para esto, se retoma la variable mencionada anteriormente, que junto a las coordenadas *i,j*, será de ayuda a la hora de seleccionar las filas y columnas a pintar de la imagen. Dado que *i,j* sí recorren la imagen en su totalidad pero no muestran los índices necesarios como para ir pintando de 100 en 100 filas y columnas, es ahí donde entra en juego la operación siguiente:

```chess_img[i*cuadrado:(i+1)*cuadrado, j*cuadrado:(j+1)*cuadrado]=255```

Entiendo *i,j* como alto y ancho de la imagen, respectivamente. Con la operación anterior se van seleccionando las filas desde *i·100* hasta *(i+1)·100* y las columnas *j·100* hasta *(j+1)·100*. Por lo que efectivamente, se pintan cuadrados de 100x100 píxeles.

### Tarea 2: Crear una imagen estilo Mondrian

En este caso se propone crear una imagen tipo [obra](https://historia-arte.com/obras/mondrian-composicion-en-rojo-amarillo-y-azul) de Piet Mondrian. Para ello, se crea primero una imagen a color de 200x300 píxeles inicializada a negro, donde posteriormente se seleccionarán las filas y columnas a rellenar para intentar la réplica de Mondrian.

```color_img=np.zeros((alto, ancho, 3), dtype = np.uint8)```

Nótese, que la imagen se crea con tres canales (RGB) en lugar de uno, lo que indica que se trata de una imagen a color. Une vez aclarado esto, se proporciona un ejemplo para pintar una de las partes de la imagen a rojo:

```color_img[0:125, 0:150, 0]=255```

Aclarar que se selecciona desde la fila 0 hasta la 124 y las columnas desde la 0 a la 149, para pintarlas a rojo. Esto se puede deducir por el tercer índice, el cual corresponde al canal rojo de la imagen.

### Tarea 3: Resolver una de las tareas anteriores con las funciones de dibujo de OpenCV

Se elige la Tarea 2, como víctima para utilizar las funciones de dibujo de OpenCV. Antes de empezar a dibujar, es necesario realizar una limpieza de imagen, ya que, se va a utilizar la que se creó anteriormente como el lienzo sobre el que usar las funciones de OpenCV. Esto se consigue cambiando los valores de la imagen en todos sus canales (RGB) a 255, lo cual, la pinta por completo de blanco:

```color_img[:, :, X]=255```

Cambiar el valor de toda la imagen a 255 en el canal X, donde X está comprendido entre 0 y 2.

Una vez hecho esto, se emplean las funciones *cv2.rectangle(img, p1, p2, color, thickness)* y *cv2.line(img, p1, p2, color, thickness)*, que sirven para dibujar rectángulos y líneas como sus nombres indican. Estas funciones reciven como parámetros: una imagen, un punto inicial (x1, y1), un punto final (x2, y2), un escalr que representa el color RGB, y un entero que supone el grosor del borde de cada figura, a continuación se muestran ejemplos:

```cv2.rectangle(color_img,(50,0),(90,50),(255,0,0),-1)```
```cv2.line(color_img,(50,0),(50,alto),(0,0,0),3)```

Se pinta un rectágulo cuyo primer vértice se encuentra en (50, 0) y su último en (90, 50), se dibuja también una línea que va desde el punto (50, 0) al (50, 200). Recalcar que primero se deben pintar los rectángulos y finalmente las líneas, ya que, solo así se consigue que encajen bien.

### Tarea 4: Modifica de forma libre los valores de un plano

Se entrega un fragmento de código cuyo propósito principal es capturar la imagen de una cámara web, para luego mostrar los tres canales RGB concatenados uno al lado del otro. En esta tarea se pide cambiar los valores asignados a cualquiera de los tres canales RGB. Se ha decido invertir el canal rojo, para ello se raliza primero la extracción de canales de los fotogramas captados de la cámara

```b = frame[:,:,0]```
```g = frame[:,:,1]```
```r = frame[:,:,2]```

Destacar que como se utiliza la librería cv2, para trabajar con el vídeo captado de la cámara, los planos de colores quedan invertidos, ya que, esta librería los entiende así. Es decir, con cv2 RGB --> BGR.

En cuanto al tema de invertir los colores, la operación a realizar se deduce de las imágenes en escala de grises. Si en una de estas imágenes, donde 255 representa el blanco y 0 el negro, se invirtiese el color. El valor 255 pasaría a representar el negro y el 0 al blanco. Por tanto, se puede deducir que si al valor máximo posible se le resta el valor que se quiere invertir se obtendrá el color inverso, abajo se relata un ejemplo

Se quiere invertir el color X en dos casos:
Caso 1:
- X = 255
  - X = 255 - 255 --> X = 0

Caso 2
- X = 0
  - X = 255 - 0 --> X = 255

Por tanto, para invertir el canal rojo, se emplea la expresión:

```r = 255 - r```

Ahora simplemente queda concatenar los canales, tal y como se hace en el código que se entrgó y mostrar el fotograma:

```collage = np.hstack((r, g, b))```
```cv2.imshow('RGB', cv2.resize(collage, (int(w*1.5),int(h/2)),cv2.INTER_NEAREST))```

### Tarea 5: Pintar círculos en las posiciones del píxel más claro y más oscuro ¿Si quisieras hacerlo sobre la zona 8x8 más clara/oscura?



### Tarea 6:


