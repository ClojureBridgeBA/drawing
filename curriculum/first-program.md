Creando tu Primer Programa con Quil
===================================

Ahora que conocés un poco acerca de como programar en Clojure, vamos a
crear una app.

En order de hacer esto, tendrás que crear un *proyecto*. Aprenderás como
organizar tu projecto con *namespaces*. También aprenderás como especificar *dependencias* de tu projecto. Finalmente, aprenderás como *construir* un proyecto para crear la app.

## Crear un proyecto

Hasta ahora estuviste experimentando en un REPL. Desafortunadamente, todo el trabajo que hacés en el REPL se pierde cuando cerrás el REPL. Podés pensar que un proyecto es un "casa permanente" para tu código. Vas a usar una herramienta llamada "Leiningen" para ayudarte a crear y gestionar tu
proyecto. Para crear un nuevo proyecto, ejecutá este comando:

```clojure
lein new quil drawing
```
Esto debería crear una estructura de directorios que debería verse así:

```
drawing
├── LICENSE
├── README.md
├── project.clj
└── src
    └── drawing
        └── core.clj
```
No hay nada inherentemente especial o Clojuristico hacerca de este esqueleto
de proyecto. Es solo una convención usada por Leiningen. Vamos a usar
Leiningen para construir y ejecutar Clojure apps, y Leiningen espera que tu
app sea de esta manera. Esta es la the función de cada parte del
esqueleto:

- `project.clj` es un archivo de configuración para Leiningen. El mismo ayuda a
  Leiningen contestar preguntas como, "Que dependencias tiene este 
  projecto?" and "Cuando este programa en Clojure ejecuta, cual función
  debe ser ejecutada primero?
- `src/drawing/core.clj` es donde va el código en Clojure

Esto usa una componente Clojure, [Quil](https://github.com/quil/quil), que crea dibujos llamados
sketches.

Ahora vamos a ir hacia adelante y ejecutar el boceto de Quil. Abrí Nightcode
y Importá - buscá la carpeta drawing y clickea. Abrí el archivo `src/drawing/core.clj`

En el fondo del lado derecho:

1. hacé click en Run with REPL
2. hacé click en Reload File

Run with REPL puede tardar un rato en arrancar. Una vez que veas el prompt, `user=>`, en el fondo de la ventan, podes hacer click en Reload.

Una ventana pop-up aparecerá con un círculo rebotando en cada pared.

Podes cerrar la pop-up clickeando el icono cerrar (X) arriba a la izquierda.


## Modificar el Proyecto

vamos a crear otro boceto de Quil. En Nightcode, elejí drawing en el lado izquierdo del árbol de directorios. 
Clickeá en New File on the top of right side window.


![Crear un nuevo archivo](images/create-new-file.png)

Nombralo como `lines.clj`.

## Organización

A medida que sus programas se vuelven más complejos, vas a tener que organizarlos. Organizá tu código Clojure poniendo related funciones and datos relacionados en archivos separados. Clojure espera que cada archivo corresponda a un
*namespace*,  entonces tenés que *declarar* un namespace al comienzo de cada archivo.

Hasta ahora, tu no te has preocupado sobre los namespaces.
Namespaces te permiten definir nuevas funciones y estructuras de datos sin preocuparte hacerca del nombre que elejíste ya fue utilizado. Por ejemplo, podes crear una función llamada `println` con un namespace personalizado
`mi-namespace-especial`, y esto no interferirá con la funciòn de Clojure `println` de la biblioteca estandart. También podes usar *un nombre completo*
`my-special-namespace/println` para distinguir tu función del `println` estandart.

Creá un namespace en el archivo `src/drawing/lines.clj`.
Abrilo, y tipeá lo siguiente:

```clojure
(ns drawing.lines)
```

Esta línea establece que todo lo que definas en este archivo va a ser guardadoadentro del  `drawing.lines` namespace.

Antes de avanzar, clickeá Save en la parte superior de la barra de menú.


## Dependencias

La parte final de trabajar con proyectos es manejar sus *dependencias*. Dependencias son solo bibliotecas de código que otros han escrito ty las cuales podemos agregar a nuestro propio proyecto.

Para agregar una dependencia, abrí `project.clj`. Deberías ver una sección con ...

```clj
:dependencies [[org.clojure/clojure "1.8.0"]
               [quil "2.4.0"]])
```

Es aquí donde las dependencias son listadas. Todas las dependencias que necesitamos para este projecto aquí son incluidas.

En orden de usar estas librerías, Vamos a tener que  _requerirlos_ en nuestro propio proyecto. En `src/drawing/lines.clj`, editá el ns statement que tipeaste anteriormente:

```clojure
(ns drawing.lines
   (:require [quil.core :as q]))
```

Esto nos da acceso a la biblioteca que necesitamos para hacer proyecto.

Hay un par de cosas para ir haciendo. Primero, el `:require` en
`ns` le dice a  Clojure de cargar otros namespaces. El `:as` del 
`:require` crea un *alias* para el namespace, dejando que te refieras a su definición sin que se haya tipeado todoel namespace. Por ejemplo, podes usar  `q/fill`  en vez de `quil.core/fill`.

Antes de avanzar, No te olvides de guardar el archivo.
Clickeá Save en la parte superior de la barra de menú.

## Tu primer programa real

### Dibujando con Quil

Quil biblioteca de Clojure provee el uso de otro biblioteca llamada [Processing](https://processing.org/), una
herramienta que te permite crear dibujos y animaciones. Vamos a usar las funciones de Quil para crear nuestros propios dibujos.

vamos a definir nuestras funciones, como ...

```clojure
(defn draw []
   ; Hacer algunas cosas ...
   )
```

... eso llama a funciones que nos provee Quil, como ...

```clojure
   ; llama a la función background de quil
   (q/background 240)
```

Poniendo todo junto:
```clojure
(defn draw []
   ; llama a la función background de quil
   (q/background 240)
   )
```

En orden de crear un dibujo (o un boceto en el lunfardo de Quil) con Quil, tenés que definir ellas funciones `setup`, `draw`, and `sketch`. `setup` es donde seteas el escenario para tu dibujo. `draw` happens repeatedly,
Estonces es ahi donde la acción de tu dibujo susede. `sketch` es el escenario en si mismo. Vamos a definir estas funciones juntos y vos vas a poder ver lo que hemos hecho.

En Nightcode, en el archivo lines.clj , agregá lo siguiente despues del paréntesis cerrado del ns de la decalración anterior.

```clojure
(defn setup []

  (q/frame-rate 30)

  (q/color-mode :rgb)

  (q/stroke 255 0 0))
```

Esta es la función de `seteo` que setea el escenario para dibujar.

Primero, vamos a llamar la función de quil `frame-rate` dejar seteado que se debe redibujar 30  veces por segundo. 
Ponemos `q/` al frente para decir que es el  `frame-rate` desde quil. Miremos esta declaración ns.
Desde que la llamamos así `:as q`, podemos usar a q como la version corta para 
quil, and `library-name/function-name` es la manera de llamar a la función desde la biblioteca.

Después, vamos a setear el modo del color a RGB.

Por último, vamos a setear el color de las lineas que vamos a dibujar con `stroke`. El código 255 0 0 representa el rojo. Podés [mirar los códigos RGB](http://xona.com/colorlist/) para ver otros colores si queres probar con algún otro.

En Nightcode, en el archivo lines.clj, agregá lo siguiente después del paréntesis en la función de seteo.

```clojure
(defn draw []

  (q/line 0 0 (q/mouse-x) (q/mouse-y))

  (q/line 200 0 (q/mouse-x) (q/mouse-y))

  (q/line 0 200 (q/mouse-x) (q/mouse-y))

  (q/line 200 200 (q/mouse-x) (q/mouse-y)))
```

Aquí llamamos la función `line` cuatro veces. También podemos llamar
a las dos funciones repetidamente con los argumentos de la función `line`:
`mouse-x` y `mouse-y`. These get the current position (x and y
coordinates on a 2d plane) del mouse. La función `line` tiene cuatro argumentos - dos pares de coordinadas x, y . la primer par x e y son la posición de comienzo de la línea. El segundo par x e y son el final de la posición de la línea. Entonces cuando empecemos cada una de las lineas va a tener un lugar fijo.

```clojure
(q/defsketch hello-lines

  :title "Podes ver las líneas"

  :size [500 500]

  :setup setup

  :draw draw

  :features [:keep-on-top])
```

Este es tu boceto. Podes setear los atributos del boceto como 
title (título) y size (tamaño). Tambien podes decir cuales son los nombres para las funciones setup y draw. Estos tienen que ser exactamente iguales a los nombres de funciones que usamos anteriormente. La última línea es para la ventana de nuestra app de dibujo esté arriba de todo.

Now click - Run with REPL - Reload File - con el cual evalua el archivo.
Tu dibujo deberia aparecer.

Si no, probá hacer - Save file - Stop - Run with REPL - Reload File.


### Ejercicio: Líneas de Arco Iris

Atualizá tu dibujo para que:

* El líneas sean de diferente color
* El título sea diferente
* El lines empiecen en un lugar diferente

Bonus: Hacer cada una de las líneas de diferente color.

Bonus #2: Cambiar el color de cada una de las líneas basada en la posición del mouse.

Pista: Podés navegar la [Quil API](http://quil.info/api) para sacar ideas y las definición de las funciones.

Pista: Podrias pensar que esto es útil: La [Quil Cheatsheet](https://github.com/ClojureBridge/curriculum/blob/gh-pages/outline/cheatsheet-quil.md) para ver las APIs selecionadas para el ClojureBridge curriculum.
