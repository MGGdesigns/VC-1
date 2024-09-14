# Visión por Computador: Práctica 1. Primeros Pasos con OpenCV

## Introducción
Esta primera práctica tiene como objetivo, famialiarizarse con las herramientas de desarrollo [Anaconda](https://www.anaconda.com/), el manejador de paquetes que se empleará para realizar las instalaciones de los paquetes necesarios durante el desarrollo de las sesiones prácticas de la asignatura. Además de comprender y saber hacer operaciones básicas de tratamiento de imágenes, como acceder a los valores de un píxel en concreto, crear una imagen, dibujar primitivas gráficas sobre una imagen, abrir una imagen de disco, acceder a los fotogramas de un vídeo, entre otros. Para esto, se dispone de una serie de tareas, que se comentan más abajo.

## Paquetes necesarios

Esta práctica está centrada mayormente en el tratamiento de imágenes, por lo que se necesitan los paquetes:
- opencv-python
- matplotlib

Para instalarlos, usar los comandos:

```pip install opencv-python```
```pip install matplotlib```

## Tareas

### Tarea 1: Crear una imagen 800x800 píxeles, con la textura de un tablero de ajedrez

Para llevar a cabo esta tarea, se establece una imagen del tamaño pedido en escala de grises inicializada a 0, es decir en negro. Seguidamente se inicializa la variable *cuadrado* a 100 píxeles, esta representará el tamaño de los cuadrados del tablero y será de ayuda a la hora de pintar los cuadrados blancos. La filosofía escogida para recorrer la imagen se ha centrado en tratarla como si de un tablero real de ajedrez se estuviese hablando. Es decir, mediante sus 8 filas y 8 columnas, lo cual nos deja con las coordenadas o posiciones *i,j*, que se usaran para determinar si el cuadrado debiese ser coloreado o no. Esto se determina al realizar el módulo 2 de la suma de ambas coordenadas, como se muestra en la operación:

```(i+j) % 2```

En el caso que su resultado sea 0, estamos ante uno de los varios cuadrados del tablero que deberían ser pintados de blanco. Para esto, se retoma la variable mencionada anteriormente, que junto a las coordenadas *i,j*, será de ayuda a la hora de seleccionar las filas y columnas a pintar de la imagen. Dado que *i,j* sí recorren la imagen en su totalidad pero no muestran los índices necesarios como para ir pintando de 100 en 100 filas y columnas, es ahí donde entra en juego la operación siguiente:

```chess_img[i*cuadrado:(i+1)*cuadrado, j*cuadrado:(j+1)*cuadrado]=255```

Entendiendo *i,j* como alto y ancho de la imagen, respectivamente. Con la operación anterior se van seleccionando las filas desde *i·100* hasta *(i+1)·100* y las columnas *j·100* hasta *(j+1)·100*. Por lo que efectivamente, se pintan cuadrados de 100x100 píxeles.

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

Una vez hecho esto, se emplean las funciones *cv2.rectangle(img, p1, p2, color, thickness)* y *cv2.line(img, p1, p2, color, thickness)*, que sirven para dibujar rectángulos y líneas como sus nombres indican. Estas funciones reciven como parámetros: una imagen, un punto inicial (x1, y1), un punto final (x2, y2), un escalar que representa el color RGB, y un entero que supone el grosor del borde de cada figura, a continuación se muestran ejemplos:

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
  - X = 255 - X --> X = 0

Caso 2
- X = 0
  - X = 255 - X --> X = 255

Por tanto, para invertir el canal rojo, se emplea la expresión:

```r = 255 - r```

Ahora simplemente queda concatenar los canales, tal y como se hace en el código que se entrgó y mostrar el fotograma:

```collage = np.hstack((r, g, b))```
```cv2.imshow('RGB', cv2.resize(collage, (int(w*1.5),int(h/2)),cv2.INTER_NEAREST))```

### Tarea 5: Pintar círculos en las posiciones del píxel más claro y más oscuro ¿Si quisieras hacerlo sobre la zona 8x8 más clara/oscura?

Para realizar esta tarea, es necesario obtener las dimensiones de la cámara web que se esté utilizando. Esto se logra con el método *get()* y las propiedades *CAP_PROP_FRAME_WIDTH* y *CAP_PROP_FRAME_HEIGHT* de la librería cv2. Una vez obtenidas las dimensiones, ya es posible recorrer la captura obtenida a través de la cámara web, en este caso se pide encontrar los píxeles en grupos de 8x8, por tanto, el rango de valores a recorrer en *i* (alto) y *j* (ancho) serán:

```for i in range(0, h - 8, 8)```
    ```for j in range(0, w - 8, 8)```

Donde *h* se corresponde con el alto de la cámara y w con su ancho. Al apreciar la expresión, se puede ver como se va saltando de 8 en 8 elementos, lo cual explica la resta que se aplica a *h* y *w*, evitando así salirse de los límites de la imagen.

Con esto aclarado solo queda mencionar, cómo se encontrarán los grupos de píxeles más claros y oscuros, para ello, se establecen las variables: *x_max*, *y_max*, *x_min* e *y_min*, todas inicializadas a 0, las cuales representarán el grupo más claro de píxeles y el más oscuro respectivamente. A parte de estas, es necesario establecer también dos variables adicionales que representan el brillo mínimo teórico y el máximo teórico:

```min_brightness = 0```
```max_brightness = 255 * 3 * 64```

El brillo mínimo que puede tener un grupo de 8x8 píxeles es teniendo todos sus canales al valor mínimo posible (negro), por tanto 0. En cuanto al brillo máximo, cada píxel dispone de 3 canales, BGR (usando cv2), el valor máximo posible que puede tener cada canal es 255. Por tanto, el brillo máximo de un único píxel vendrá dado por la multiplicación de 255 por tres, sin embargo, se está tratando con un grupo de 8x8 píxeles. Dicho de otro modo se trata con 64 píxeles, de ahí que se añada la multiplicación por 64 en la inicialización del brillo máximo teórico.

Habiendo aclarado esto, en cada iteración del bucle anterior se deben obtener los grupos de 8x8 del fotograma actual, para posteriormente calcular el brillo máximo del grupo. En este caso, realizando la suma de los valores de cada canal, ya que, no se puede asumir el valor ideal de 255:

```pixelGroup = frame[i:i+8, j:j+8]``` --> Selección del grupo de píxeles
```pixel_colour_sum = np.sum(pixelGroup[:, :, 0]) + np.sum(pixelGroup[:, :, 1]) + np.sum(pixelGroup[:, :, 2])``` --> Cálculo del brillo máximo del grupo

Una vez se ha obtenido el brillo máximo del grupo, se debe comparar con el brillo máximo y mínimo teóricos

Caso 1:
- pixel_colour_sum < max_brightness
  - El brillo del grupo es menor al máximo teórico, por tanto, se establece este nuevo brillo como máximo teórico y se actualizan *x_min* e *y_min* con *i* y *j* que representan el grupo de píxeles 8x8

Caso 2:
- pixel_colour_sum > min_brightness
  - El brillo del grupo es mayor al mínimo teórico, por tanto, se establece este nuevo brillo como mínimo teórico y se actualizan *x_max* e *y_max* con *i* y *j* que representan el grupo de píxeles 8x8

Tras esto simplemente queda representar los círculos, para ello, se utiliza el comando *cv2.circle(img, center, radious, color, thickness)*. Este comando recibe como parámetros: una imagen, el punto que será el centro del círculo, el radio del círculo, un escalar que represenata el color en BGR y un entero que supone el grosor del círculo.

```cv2.circle(frame, (x_max, y_max), 8, (0, 0, 255), 3)```
```cv2.circle(frame, (x_min, y_min), 8, (255, 0, 0), 3)```

Se pintan, a partir del fotograma actual, los círculos que comprenden a los grupos de 8x8 píxeles más claros/oscuros, siendo los claros representados por el color rojo y los más oscuros por el azul.

### Tarea 6: Llevar a cabo una propuesta propia de pop art

Se proporciona un código para la representación de un pop art, al igual que en la Tarea 4 se propone realizar un cambio en los valores predeterminados para crear una versión propia de pop art. En el código proporcionado por el profesor se divide la pantalla en cuatro porciones, la superior izquierda *(tl)*, la superior derecha *(tr)*, la inferior izquierda *(bl)* y la inferior derecha *(br)*.

Inicialización de la imagen y las variables anteriores (h y w representan el alto y ancho de la cámara):
- collage = np.zeros((h*2,w*2,3), dtype = np.uint8)
- tl = collage[0:h, 0:w]
- tr = collage[0:h, w:w+w]
- bl = collage[h:h+h, 0:w]
- br = collage[h:h+h, w:w+w]

Los valores de los planos originales se extraen de cada fotograma:
- r = frame[:, :, 2]
- g = frame[:, :, 1]
- b = frame[:, :, 0]

Los valores predeterminados de cada variable son:

tl
- tl[:, :, 0] = b
- tl[:, :, 1] = g
- tl[:, :, 2] = r


tr
- tr[:, :, 0] = 255 - r
- tr[:, :, 1] = g
- tr[:, :, 2] = b


bl
- bl[:, :, 0] = r
- bl[:, :, 1] = 255 - b
- bl[:, :, 2] = g


br
- br[:, :, 0] = b
- br[:, :, 1] = g
- br[:, :, 2] = 255 - r

Los valores que se decidió tocar son:

tr
- tr[:, :, 0] = 255 - r
- tr[:, :, 1] = 255 - g
- tr[:, :, 2] = 255 - b


br
- br[:, :, 0] = 255 - b
- br[:, :, 1] = 255 - g
- br[:, :, 2] = r

Tras haber, modificado los valores, lo único que hace falta es mostrar el nuevo pop art.

```cv2.imshow('Cam', collage)```

### Autores

- [@Mauro Gómez Guillén](https://github.com/MGGdesigns)
- [@Santiago Santana Martínez](https://github.com/Tiago1615)

### Referencias Bibliográficas

- [Guión de la práctica](https://github.com/otsedom/otsedom.github.io/blob/main/VC/P1/README.md)
- [Idea para hallar los cuadrados del tablero de ajedrez](https://github.com/doocs/leetcode/blob/main/solution/1800-1899/1812.Determine%20Color%20of%20a%20Chessboard%20Square/README_EN.md)
- [Funciones de dibujo de primitivas OpenCV](https://docs.opencv.org/4.x/d6/d6e/group__imgproc__draw.html#ga07d2f74cadcf8e305e810ce8eed13bc9)
- [Obtener las dimensiones de la cámara web](https://stackoverflow.com/questions/39953263/get-video-dimension-in-python-opencv)
