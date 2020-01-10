¡Quiero crear algo copado con Quil!
===================================

    Esta es la historia de Clara, que hacía poco había asistido a un workshop de
    ClojureBridge.
    En el workshop aprendió lo que era Clojure y cómo escribir código en Clojure.
    Quedó realmente impresionada, ¡qué funcional!.
    Además, había conocido una herramienta para dibujar muy interesante que se
    llamaba Quil y estaba programada en Clojure.
    Cuando Clara aprendió a usar Quil, pensó, "¡quiero crear algo copado con
    Quil!"
    Así fue como Clara desarrollo su propia app con Quil.

## Paso 1. Un copo de nieve en un fondo azul

Clara estaba pensando por dónde empezar cuando de repente se le prendió la
lamparita: "un copo de nieve blanco con un fondo azul estaría muy bien".
Ahora quería saber cómo dibujarlo y ya sabía como descubrirlo:

1. Ir al website con la documentación de la API
2. Googlearlo

Inmediatamente, Clara visitó [http://quil.info/api](http://quil.info/api), donde
está documentada la API de Quil, y encontró la sección [Image > Loading and
Displaying](http://quil.info/api/image/loading-and-displaying). También encontró
la función
[background-image](http://quil.info/api/color/setting#background-image).

Después, googleó y llegó a una pregunta en StackOverflow que parecía útil,
[Load/display image in clojure with
quil](http://stackoverflow.com/questions/18714941/load-display-image-in-clojure-with-quil).
Además revisó la sección de recursos de ClojureBridge [Quil and Processing
Resources](https://github.com/ClojureBridge/drawing#quil-and-processing-resources).
Hojeando los web sites que apareceren ahí, descubrió un ejemplo prometedor,
[xmas-tree.clj](https://github.com/quephird/fun-with-quil/blob/master/src/fun_with_quil/animations/xmas-tree.clj),
que carga un archivo de una imagen con un árbol de navidad y la muestra en una ventana.
"Esto debería ser lo que quiero", pensó, encantada con los resultados de su búsqueda.

Ahora tenía toda la información que necesitaba para completar el primer paso.

En el workshop de ClojureBridge, Clara había completado el tutorial, [Making
Your First Program with
Quil](https://github.com/ClojureBridge/drawing/blob/master/curriculum/first-program.md),
por lo que ya tenía el proyecto de Clojure `drawing` en su computadora. Decidió
usar el mismo ptoyecto para su app para no tener que crear uno nuevo.

### Paso 1-1: Crear un nuevo archivo fuente

Clara agregó un nuevo archivo dentro del directorio `src/drawing` con el nombre 
`practice.clj`. A esta altura, sus archivos se ven así:

```
drawing
├── LICENSE
├── README.md
├── project.clj
└── src
    └── drawing
        ├── core.clj
        ├── lines.clj
        └── practice.clj      <-- se agrega en el paso 1-1
```

### Paso 1-2: Agregar el namespace — hacer el código fuente clojurístico

Al principio de un archivo fuente de Clojure se encuentra la declaración del
namespace. Clara, copió y pegó la forma `ns` que estaba arriba de todo en el
archivo `lines.clj`. En seguida, cambió el nombre de `drawing.lines` a
`drawing.practice`, porque el nuevo archivo se llama `practice`.

En este momento, `practice.clj` se ve así:

```clojure
(ns drawing.practice
  (:require [quil.core :as q]))
```

### Paso 1-3: Agregar el código básico de Quil

Las funciones `setup` y `draw`, junto con la macro `defsketch`, que define la
app, necesitan estar presentes en cualquier código que use Quil. Por esta razón,
Clara las agregó a `practice.clj`, que ahora quedó ve así:

```clojure
(ns drawing.practice
  (:require [quil.core :as q]))

;; funciones setup y draw, y q/defsketch se agregan en el paso 1-3
(defn setup [])

(defn draw [])

(q/defsketch practice
  :title "Clara's Quil practice"
  :size [500 500]
  :setup setup
  :draw draw
  :features [:keep-on-top])
```

### Paso 1-4: Descargar las imágenes del copo de nieve y el fondo

Leyendo la API de Quil y la pregunta en StackOverlow, Clara aprendió que el
lugar dónde se guaraban las imágenes era importante. Entonces, creó un nuevo
directorio, llamado `images`, dentro del directorio principal `drawing`, y
[descargó las imágenes ahí](https://github.com/ClojureBridge/drawing/tree/master/images).

Ahora, sus archivos quedaron así:

```
drawing
├── LICENSE
├── README.md
├── images
│   ├── blue_background.png      <-- se agrega en el paso 1-4
│   └── white_flake.png          <-- se agrega en el paso 1-4
├── project.clj
└── src
    └── drawing
        ├── core.clj
        ├── lines.clj
        └── practice.clj
```

### Paso 1-5: Habilitar el modo funcional

Ahora que las imágenes estaban listas, el próximo paso era usarlas a través de
la API de Quil.

Clara volvió al ejemplo del árbol de navidad y se dió cuenta que su app
necesitaba habilitar el modo funcional — fue toda una experiencia. Para
descifrar exactamente qué era y como usarlo, Clara recorrió el documento
[Functional mode (fun mode)](https://github.com/quil/quil/wiki/Functional-mode-(fun-mode)).

Cuando terminó de leer el documento, murmuró "¡Ja! Fun mode, ¡que nombre
gracioso! (_fun_ significa _divertido _en Inglés). Entonces... lo que tengo que
hacer es agregar:"

1. `[quil.middleware :as m]` en el `:require` de la forma `ns`,
2. `:middleware [m/fun-mode]` en la forma `q/defsketch`, y
3. el argumento `state` a la función `draw`

"a mi archivo `practice.clj`. Ok, ¡puedo hacerlo!". Y su archivo `practice.clj`
ahora se veía así:

```clojure
(ns drawing.practice
  (:require [quil.core :as q]
            [quil.middleware :as m]))  ;; esta línea se agrega en paso 1-5

(defn setup [])

(defn draw [state])                    ;; argumento state se agrega en paso 1-5

(q/defsketch practice
  :title "Clara's Quil practice"
  :size [500 500]
  :setup setup
  :draw draw
  :features [:keep-on-top]
  :middleware [m/fun-mode])             ;; esta línea se agrega en paso 1-5
```

### Paso 1-6: Cargar y mostrar las imágenes del copo de nieve y el fondo

Clara ya sabía cuáles eran las últimas piezas que faltaban para poder cargar y
mostar las imágenes:

1. En la función`setup`, cargar las imágenes y devolverlas en el **estado**.
2. En la función `draw`, obtener las imágenes del estado (mirando el contenido
   del argumento `state`) y dibujarlas.

Entonces, agregó un par de líneas de código a las funciones `setup` y `draw` en
su `practice.clj`. Tuvo mucho cuidado cuando escribió los nombres de los
archivos de las imágenes porque deben coincidir exactamente y reflejar la
estructura de directorios.

### `practice.clj` al final del paso 1

A esta altura, `practice.clj` se veía así:

```clojure
(ns drawing.practice
  (:require [quil.core :as q]
            [quil.middleware :as m]))

(defn setup []
  ;; el mapa (estructura de datos) de estas dos líneas se agregan en paso 1-6
  {:flake (q/load-image "images/white_flake.png")
   :background (q/load-image "images/blue_background.png")}
  )

(defn draw [state]
  ;; funciones q/background-image y q/image se agregan en paso 1-6
  (q/background-image (:background state))
  (q/image (:flake state) 200 10)
  )

(q/defsketch practice
  :title "Clara's Quil practice"
  :size [500 500]
  :setup setup
  :draw draw
  :features [:keep-on-top]
  :middleware [m/fun-mode])
```

Cuando Clara ejecutó este código, pudo ver esta imagen:

![step 1 screenshot](images/step-1.png)

Vaamoos! Lo logró!


#### [Bonus] Desestructuración

Clojure tiene una funcionalidad muy copada que se llama desestructucturación o 
[destructuring](http://clojurebridge.github.io/community-docs/docs/clojure/destructuring/)
en Inglés.
Si se usa la desestructucturación en el argumento de la función `draw` podemos
reescribirla así:

```clojure
(defn draw [{flake :flake background :background}]
  (q/background-image background)
  (q/image flake 200 10))
```

Esta funcionalidad es basante útil y frecuentemente utilizada por Clojuristas.


## Step 2. Copo de nieve cayendo al piso

Clara estaba contenta con la imagen del copo de nieve blanco sobre el fondo
azul, pero le empezó a parecer aburrida. Para el próximo paso quería mover el
copo de nieve como si estuviera cayendo al piso. Para esto necesitaba seguir
estudiando la API de Quil y googlear un poco más.

Encontró que a mover piezas en una imagen se le llama **animación**, y que
básicamente la idea es:

- dibujar la imagen en alguna posición,
- actualizar la posición, y
- dibujar otra vez la imagen, pero en la nueva posición.

La animaciones repiten estos pasos una y otra vez.

### Paso 2-1: Agregar actualización del parámetro `y`

"Bueno", pensó clara, "¿qué significa 'mover el copo de nieve como si estuviera
cayendo al piso' desde el punto de vista de la programación?".

Para dibujar el copo de nieve, había usado la función `image` de Quil, explicada
en la documentación de la API:
[image](http://quil.info/api/image/loading-and-displaying#image).

Los valores que eligió para los parámetros `x` e `y` fueron 200 y 10 medidos
desde la esquina superior-izquierda, porque esa era la posición en la que quería
dibujar el copo de nieve. Para hacerlo caer, el parámetro `y` debería
incrementarse a medida que pasa el tiempo.

![x e y](images/x-y-grid.png)

Desde el punto de vista de la programación, 'mover el copo de nieve como si
estuviera cayendo al piso', significa:

1. Definir el estado inicial — por ejemplo, `(x, y) = (200, 10)`.
2. Dibujar el fondo primero, y después el copo de nieve.
3. Actualizar el estado incrementando el parámetro `y` — por ejemplo, `(x, y) = (200, 11)`.
4. Volver a dibujar el fondo primero, y después el copo de nieve.
5. Repetir los puntos 3 y 4.

En su app, el único estado que necesita cambiar es el parámetro `y`. ¿Cómo hizo
para incrementar en uno el valor de `y`? Clojure tiene la función `inc`, que es
exactamente la función que usó.

Para actualizar el parámetro `y`:
- Agregar un parámetro `y` inicial en el mapa de la función `setup`, que
  representa el **estado**.

    ```clojure
    {:flake (q/load-image "images/white_flake.png")
     :background (q/load-image "images/blue_background.png")
     :y-param 10}
    ```

-  Agregar una nueva función `update` que incrementa en uno el valor del
   parámetro `y`.

    ```clojure
    (defn update [state]
      ;; actualizando el parámetro y, incrementando en 1
      (update-in state [:y-param] inc))
    ```

-  Agregar la función `update` en la forma `q/defsketch`.

    ```clojure
    (q/defsketch practice
      :title "Clara's Quil practice"
      :size [500 500]
      :setup setup
      :update update
      :draw draw
      :features [:keep-on-top]
      :middleware [m/fun-mode])
    ```

### Paso 2-2: Dibujar la imagen en la posición actualizada

Si bien la app ahora tiene la funcionalidad de actualizar el parámetro `y`, no
le alcanza para hacer que el copo de nieve caiga. El copo de nieve se tiene que
dibujar en la nueva posición actualizada.

Clara cambió la función `draw` para que `q/image` use el parámetro `y`
actualizado.

```clojure
(defn draw [state]
  ;; dibujando un fondo azul con un copo de nieve arriba
  (q/background-image (:background state))
  (q/image (:flake state) 200 (:y-param state)))
```

Además, agregó una función más a `setup`, `(q/smooth)`, para que la animación
sea más fluida.

### `practice.clj` al final del paso 2

A esta altura, `practice.clj` se veía así:

```clojure
(ns drawing.practice
  (:require [quil.core :as q]
            [quil.middleware :as m]))

(defn setup []
  (q/smooth)                                      ;; se agrega en paso 2-2
  {:flake (q/load-image "images/white_flake.png")
   :background (q/load-image "images/blue_background.png")
   :y-param 10}                                   ;; se agrega en paso 2-1
  )

;; función update se agrega en paso 2-1
(defn update [state]
  (update-in state [:y-param] inc))

(defn draw [state]
  (q/background-image (:background state))
  (q/image (:flake state) 200 (:y-param state))    ;; se cambia en paso 2-2
  )

(q/defsketch practice
  :title "Clara's Quil practice"
  :size [500 500]
  :setup setup
  :update update                                   ;; se agrega en paso 2-1
  :draw draw
  :features [:keep-on-top]
  :middleware [m/fun-mode])
```

Cuando Clara ejecutó este código, pudo ver como el copo de nieve caia al piso —
genial!


## Paso 3. Hacer que copo de nieve siga cayendo una y otra vez

Clara tenía una linda app Quil, pero se terminaba muy rápido. Una vez que el
copo de nieve pasaba el borde inferior de la ventana, solo quedaba el fondo
azul. Por eso, ella quería que se repita, una y otra vez.

En otras palabras: si el copo de nieve llega al fondo debería reaparecer por la
parte superior y volver a caer.

¿Qué significa desde el punto de vista de la programación?"

Si el valor del parámetro `y` es mayor que el alto de la imagen, entonces su
valor debe volver a `0`. Si no lo es, su valor debe incrementarse en uno. Ahora
existen dos formas de actualizar el parámetro `y`.


Clara se acordó que en workshop de ClojureBridge había usado `if`
[Flow Control](http://clojurebridge.github.io/curriculum/outline/flow_control.html).
 Parecía que con `if` iba a poder elegir entre los dos casos.

```clojure
(defn update [state]
  (if (>= (:y-param state) (q/height)) ;; y-param es igual o mayor que el alto de la imagen?
    (assoc state :y-param 0)           ;; si - vuelve a 0 (arriba)
    (update-in state [:y-param] inc)   ;; no - incrementar en 1
    ))
```

Y así fue como Clara uso `if` para hacer que el copo de nieve vuelva arriba de
todo en la función `update`.

### `practice.clj` al final del paso 3

A esta altura, `practice.clj` se veía así:

```clojure
(ns drawing.practice
  (:require [quil.core :as q]
            [quil.middleware :as m]))

(defn setup []
  (q/smooth)
  {:flake (q/load-image "images/white_flake.png")
   :background (q/load-image "images/blue_background.png")
   :y-param 10})

(defn update [state]
  ;; estas tres líneas se agregan en el paso 3
  (if (>= (:y-param state) (q/height))
    (assoc state :y-param 0)
    (update-in state [:y-param] inc)))

(defn draw [state]
  (q/background-image (:background state))
  (q/image (:flake state) 200 (:y-param state)))

(q/defsketch practice
  :title "Clara's Quil practice"
  :size [500 500]
  :setup setup
  :update update
  :draw draw
  :features [:keep-on-top]
  :middleware [m/fun-mode])
```

Cuando Clara ejecutó este código, pudo ver al copo de nieve aparecer por arriba
después de desaparecer por abajo.

## Paso 4. Agregar más copos de nieve cayendo

Clara pensó, "es lindo como cae el copo de nieve una y otra vez, pero tengo uno
solo, ¿puedo agregar más?". Ella quería ver más copos de nieve cayendo: uno o
más en la mitad izquierda, y uno o más en la mitad derecha.

Una vez más, necesitaba expresar sus pensamientos usando palabras del mundo de
la programación. Mirándo al código que había escrito, se da cuenta que se
traduciría a "dibujar varias imágenes con distintos valores para el parámetro
`x`". La forma más simple sería copiar y pegar `(q/image (:flake state) 200
(:y-param state)` varias veces con diferentes valores de `x`. Por ejemplo:

```clojure
(q/image (:flake state) 10 (:y-param state))
(q/image (:flake state) 200 (:y-param state))
(q/image (:flake state) 390 (:y-param state))
```

Pero para clara, esto no se veía bien; había aprendido un montón sobre Clojure y
quería ponerlo en práctica.

Pensando en cómo mantener registro de varios parámetros `x` se acordó de los
vectores que vió cuando aprendió sobre [Estructuras de
Datos](http://clojurebridge.github.io/curriculum/outline/data_structures.html) y
le parecieron una buena idea.

Esto es lo que hizo para agregar más copos de nieve:

1. Agregar un vector, con varios parámetros `x`, y asignarle un nombre con
   `def`.

    ```clojure
    (def x-params [10 200 390]) ;; parámetros x para 3 copos de nieve
    ```

2. Dibujar varios copos de nieve, uno por cada parámetro `x`, usando `doseq`.

    ```clojure
    (doseq [x x-params]
      (q/image (:flake state) x (:y-param state)))
    ```

* Para recordar como funciona `doseq` podés consultar la
  [currícula](http://clojurebridge.github.io/curriculum/outline/sequences.html#/3).

### `practice.clj` al final del paso 4

A esta altura, `practice.clj` se veía así:

```clojure
(ns drawing.practice
  (:require [quil.core :as q]
            [quil.middleware :as m]))

(def x-params [10 200 390])                      ;; se agregó en el paso 4

(defn setup []
  (q/smooth)
  {:flake (q/load-image "images/white_flake.png")
   :background (q/load-image "images/blue_background.png")
   :y-param 10})

(defn update [state]
  (if (>= (:y-param state) (q/height))
    (assoc state :y-param 0)
    (update-in state [:y-param] inc)))

(defn draw [state]
  (q/background-image (:background state))
  (doseq [x x-params]                              ;; dos líneas se cambiaron
    (q/image (:flake state) x (:y-param state))))  ;; en el paso 4

(q/defsketch practice
  :title "Clara's Quil practice"
  :size [500 500]
  :setup setup
  :update update
  :draw draw
  :features [:keep-on-top]
  :middleware [m/fun-mode])
```

"Si!" Clara gritó al ver tres copos de nieve cayendo una y otra vez.


## Paso 5. Hacer que los copos de nieve caigan a distintas velocidades

A pesar de que su app se veía encantadora, Clara no podía evitar sentir que algo no
estaba del todo bien. En su ventana, los tres copos de nieve caían a la misma
velocidad, como si estuvieran en una marcha de robots. No se sentía natural. Por
esto quiso hacer que cada uno caiga a una velocidad distinta.

Usando términos de programación, el problema es que los tres copos de nieve
comparten el mismo parámetro `y`. Varios parámetros `y` solucionarían el
problema pero, ¿cómo lo implementaríamos?

### Paso 5-1 Cambiar el parámetro `y` por mapas con velocidades

Al igual que con los parámetros `x`, un `vector` es una buena estructura de
datos para tener diferentes parámetros `y`. Sin embargo, este vector no puede
ser exactamente igual al que usó para los parámetros `x` ya que cada copo de
nieve tiene dos parámetros: la posición y la velocidad. Tener los dos parámetros
le permitiría a Clara cambiar la velocidad a la que cae cada copo de nieve.

Entonces volvió a la currícula de ClojureBridge y repasó la sección de
[Estructuras de Datos
](http://clojurebridge.github.io/curriculum/outline/data_structures.html). Ahí
había una estructura de datos llama Mapa que le permitía guardar varios
parámetros juntos. Después de ver los ejemplos de mapas, cambió `y-param` de un
valor simple (un número) a un vector de 3 mapas. Además, cambió la clave de
`y-param` a `y-params`. A partir de ese momento, su **estado** inicial se
convirtió en esto: d

```clojure
{:flake (q/load-image "images/white_flake.png")
 :background (q/load-image "images/blue_background.png")
 :y-params [{:y 10 :speed 1} {:y 150 :speed 4} {:y 50 :speed 2}]}
```

Era una buena estrucura de datos, mapas dentro de un vector que a su vez estaba
dentro de otro mapa. Lo malo era que su función `update` no iba a poder seguir
siendo simple, ahora tenía que actualizar los valores `y` de cada uno de los
tres mapas del vector.

### Paso 5-2 Actualizar valores en los mapas del vector

Pensar al mismo tiempo en mapas y vectores le resultaba confuso, entonces
decidió concentrarse solo en un mapa. Cada mapa tiene un parámetro `y` y
`speed`. Por ejemplo, el siguiente mapa podría ser parte del **estado** inicial:

```clojure
{:y 150 :speed 4}
```

Inmediatamente, la velocidad `speed` debería sumarse al valor de `y`, por lo que
el mapa debería actualizarse a:

```clojure
{:y 154 :speed 4}
```

Para lograr esto, Clara agregó la función `update-y`:

```clojure
(defn update-y
  [m]
  (let [y (:y m)
        speed (:speed m)]
    (if (>= y (q/height))           ;; y es igual o mayor que el alto de la imagen?
      (assoc m :y 0)                ;; si - vuelve a 0 (arriba)
      (update-in m [:y] + speed)))) ;; no - sumar el valor de y y speed
```

#### [Bonus] Desestructuración

Usando la desestructuración de Clojure, podemos escribir `update-y` de esta
manera:

```clojure
(defn update-y
  [{y :y speed :speed :as m}]
  (if (>= y (q/height))
    (assoc m :y 0)
    (update-in m [:y] + speed)))
```

Así nos evitamos de escribir la forma especial `let`.


### Paso 5-3 Actualizar los mapas en el vector

El siguiente paso es actualizar los mapas en el vector usando la función
`update-y`. Antes de programar a esta parte, Clara redujo el problema para
concentrarse en actualizar un vector: ¿cómo se actualizan los datos dentro un
vector?. Se acordó que existía una [función
`map`](http://clojurebridge.github.io/curriculum/outline/functions.html#/9) que
le permitía aplicar una función a cada elmento del vector.

Por ejemplo:

```clojure
(map inc [1 2 3]) ;=> (2 3 4)
```

> Clojure tiene una función `map` y una estructura de datos también llamada
> `map` (mapa).
> Ten cuidado, es confuso.
> En Python, `map` es la función, y diccionario es la estructura de datos.
> En Ruby, `map` (tamibén llamada `collect`) es la función, y hash es la
> estructura de datos.

Entonces probó la función en el REPL:

```clojure
(defn update-test
  [m]
  (let [y (:y m)
        speed (:speed m)]
    (if (>= y 500)
      (assoc m :y 0)
      (update-in m [:y] + speed))))

(def y-params [{:y 10 :speed 1} {:y 150 :speed 4} {:y 50 :speed 2}])

(map update-test y-params)
;=> ({:y 11, :speed 1} {:y 154, :speed 4} {:y 52, :speed 2})
```

Como funcionó bien, volvió a su archivo `practice.clj` para cambiar la función
`update`. Esta función debe devolver el **estado** actualizado. En su caso, lo
único que es necesario actualizar es el vector asociado a la clave `y-params`.
Así, su función `update` se convirtió en:

```clojure
(defn update [state]
  (let [y-params (:y-params state)]
    (assoc state :y-params (map update-y y-params))))
```


### Paso 5-4 Actualizar la función draw para que vea los mapas del vector

Otro desafío fue el cambio que necesitó hacerle a la función `draw` para que
pueda ver los mapas que se encontraban en el vector. Clara encontró varias
maneras de repertir algo en Clojure. Entre ellas, eligió una alternativa que
usaba `dotimes` y `nth` para dibujar imágenes repetidamente — la función `nth`
es la que se explica en la sección [Estructuras de
Datos](http://clojurebridge.github.io/curriculum/outline/data_structures.html#/6)
de la currícula.

En este caso, como sabía que había exactamente 3 copos de nieves, pudo cambiar
el código por el siguiente:

```clojure
(let [y-params (:y-params state)]
    (dotimes [n 3]
      (q/image (:flake state) (nth x-params n) (:y (nth y-params n)))))
```

### `practice.clj` al final del paso 5

A esta altura, `practice.clj` se veía así:

```clojure
(ns drawing.practice
  (:require [quil.core :as q]
            [quil.middleware :as m]))

(def x-params [10 200 390])

(defn setup []
  (q/smooth)
  {:flake (q/load-image "images/white_flake.png")
   :background (q/load-image "images/blue_background.png")
   :y-params [{:y 10 :speed 1} {:y 150 :speed 4} {:y 50 :speed 2}]})  ;; se cambió en el paso 5-1

;; función update-y se agregó en el paso 5-2
(defn update-y
  [m]
  (let [y (:y m)
        speed (:speed m)]
    (if (>= y (q/height))
      (assoc m :y 0)
      (update-in m [:y] + speed))))

(defn update [state]
  (let [y-params (:y-params state)]                   ;; función update
    (assoc state :y-params (map update-y y-params)))) ;; se cambió en el paso 5-3

(defn draw [state]
  (q/background-image (:background state))
  (let [y-params (:y-params state)]   ;; 3 líneas se cambiaron en el paso 5-4
    (dotimes [n 3]
      (q/image (:flake state) (nth x-params n) (:y (nth y-params n))))))

(q/defsketch practice
  :title "Clara's Quil practice"
  :size [500 500]
  :setup setup
  :update update
  :draw draw
  :features [:keep-on-top]
  :middleware [m/fun-mode])
```

Cuando Clara ejecutó este código, tres copos de nieve cayeron a distintas
velocidades y se veía más natural.

## Paso 6. "Refactorizar"

Clara miró a su archivo `practice.clj` pensando que su código se había vuelto
bastante largo.

Después de escanear otra vez su código de arriba a abajo, pensó "tal vez
`x-params` podría ser parte del **estado**". Por ejemplo:

```clojure
[{:x 10 :y 10 :speed 1} {:x 200 :y 150 :speed 4} {:x 390 :y 50 :speed 2}]
```

Se encontró con que esta nueva estructura de datos facilitaba la tarea de
mantener el estado de cada copo de nieve.

Para usar la nueva estructura de datos, cambió su función `setup` para que el
**estado** inicial incluya los parámetros `x`. Además, cambió el nombre de la
clave `:y-params` a `:params`:

```clojure
{:flake (q/load-image "images/white_flake.png")
   :background (q/load-image "images/blue_background.png")
   :params [{:x 10  :y 10  :speed 1}
            {:x 200 :y 150 :speed 4}
            {:x 390 :y 50  :speed 2}]}
```

Clara miró fijamente a la función `update-y` y concluyó dejarla como estaba. La
existencia de `:x` y su valor no afectaba la lógica que actualizaba el valor de
`:y`. La función devolvía el mapa con tres pares clave-valor con el valor de
`:y` actualizado.

¿Qué pasaba con la función `update`? Necesitaba un pequeño cambio dado que el
nombre de la clave cambió de `:y-params` a `:params`.

```clojure
(defn update [state]
  (let [params (:params state)]
    (assoc state :params (map update-y params))))
```

La función `draw` requería un cambio mayor porque la forma en la que se extraían
los valores de `:x` había cambiado. En un primer lugar, Clara cambió la forma
`dotimes` así:

```clojure
(let [params (:params state)]
  (dotimes [n 3]
    (q/image (:flake state) (:x (nth params n)) (:y (nth params n)))))
```

Pero la misma cosa, `(nth params n)`, aparecía dos veces. "¿Hay algo mejor para
evitar la repetición?" pensó. La respuesta era fácil — usar la forma especial
`let` dentro de la función `dotimes`.

Al usar `let`, su forma `dotimes` se quedó así:

```clojure
(let [params (:params state)]
  (dotimes [n 3]
    (let [param (nth params n)]
      (q/image (:flake state) (:x param) (:y param)))))
```

¡Esa última línea quedó mucho más limpia!

### `practice.clj` al final del paso 6

A esta altura, `practice.clj` se veía así:

```clojure
(ns drawing.practice
  (:require [quil.core :as q]
            [quil.middleware :as m]))

(defn setup []
  (q/smooth)
  {:flake (q/load-image "images/white_flake.png")
   :background (q/load-image "images/blue_background.png")
   :params [{:x 10  :y 10  :speed 1}                       ;; se cambió en el paso 6
            {:x 200 :y 150 :speed 4}                       ;;
            {:x 390 :y 50  :speed 2}]})                    ;;

(defn update-y
  [m]
  (let [y (:y m)
        speed (:speed m)]
    (if (>= y (q/height))
      (assoc m :y 0)
      (update-in m [:y] + speed))))

(defn update [state]
  (let [params (:params state)]                    ;; se cambió por params en el paso 6
    (assoc state :params (map update-y params))))  ;;

(defn draw [state]
  (q/background-image (:background state))
  (let [params (:params state)]                            ;; se cambió en el paso 6
    (dotimes [n 3]                                         ;;
      (let [param (nth params n)]                          ;;
        (q/image (:flake state) (:x param) (:y param)))))) ;;

(q/defsketch practice
  :title "Clara's Quil practice"
  :size [500 500]
  :setup setup
  :update update
  :draw draw
  :features [:keep-on-top]
  :middleware [m/fun-mode])
```

Clara no vió ningún cambio con respecto al paso 5, pero el código se veía mucho
mejor. A este tipo de trabajo se lo conoce comunmente como "refactorización".

## Step 7. Make snowflakes swing as falling down

Clara was getting much familiar with Clojure coding. Her Quil app was
getting much more fantastic, as well!

It was amusing to look at snowflakes falling down in different speeds.
But, something she didn't like was... all of the snowflakes were
falling straight down. This was much better than marching robots:
however, it would be awesome if snowflakes were swinging left and
right as falling down.

In the words of programming, the `x` parameter should either increase or
decrease when the value is updated. This means that the `update`
function should update the `x` parameters as well as the `y`
parameters so that snowflakes took a trace like this:

![curve to fall down](images/curve.png)

This would be a big challenge to her.

### step 7-1 Add swing parameter to initial **state**

Clara already knew from her experience on y value: different swing parameters
to three x values would give her nice swing motion.
She changed the initial **state** to have swing parameter like this:

```clojure
[{:x 10  :swing 1 :y 10  :speed 1}
 {:x 200 :swing 3 :y 100 :speed 4}
 {:x 390 :swing 2 :y 50  :speed 2}]
```

The `swing` parameters should work to give different ranges between
leftmost and rightmost of the curve.

![swing of curve](images/curve-range.png)


Her `setup` function became like this:

```clojure
(defn setup []
  (q/smooth)
  {:flake (q/load-image "images/white_flake.png")
   :background (q/load-image "images/blue_background.png")
   :params [{:x 10  :swing 1 :y 10  :speed 1}
            {:x 200 :swing 3 :y 100 :speed 4}
            {:x 390 :swing 2 :y 50  :speed 2}]})
```

### step 7-2 Calculate x values to swing snowflakes

Updating x values were quite similar to the one for y values.
Like Clara added the `update-y` function, she was about to write
`update-x` function. But she stopped and thought, "How can I calculate
updated x values?" sketching a curve in her mind.

She went to Quil api document and scanned the functions thinking some
might have helped her. She found
[`sin`](http://quil.info/api/math/trigonometry#sin) function which
was to calculate the sine of an angle. Also she recalled what was the shape
of sine curve.

The curve she want had roughly the shape of:

```
x = sin(y)
```

If she considered the size of window, a couple more parameters were
needed to make swing look nice, for example:

```
x = x + a * sin(y / b)
```

The parameter `a` exactly works as the `swing` she added to the **state**.
When the `swing` is 1, the curve traces like in the left image. While
the `swing` is 3, the curve becomes the right image.

![swing is 1](images/1-sin-x.png)  ![swing is 3](images/3-sin-x.png)

The parameter `b` adjusts distances between peeks. If the value is
small, the snowflake goes left and right busily. On the other hand, if
the value is big, the snowflakes moves loosely. Thinking of the size
of window, 50 would be a good number for `b`.
Given that, the update function would be:

```
x = x + swing * sin(y / 50)
```

Not just update x values, the function should handle the cases x is
smaller than 0 (too left), and x is greater than image width (too
right). When the x value goes to less than `0`, it should take the value of the
image width, so that the snowflake will appear from the right.
Likewise, when the x value goes to more than the image width, it
should have value `0` so that the snowflake will appear from the left.
Reflecting this conditions, her `update-x` function became
like this:

```clojure
(defn update-x
  [m]
  (let [x (:x m)
        swing (:swing m)
        y (:y m)]
    (cond
     (< x 0) (assoc m :x (q/width))                                  ;; too left
     (< x (q/width)) (update-in m [:x] + (* swing (q/sin (/ y 50)))) ;; within frame
     :else (assoc m :x 0))))                                         ;; too right
```

In this function, she couldn't use `if` anymore since `if` takes only one
predicate (comparison). Instead of `if`, she used `cond` which allowed
her to handle multiple comparisons.


### step 7-3 Update x and y values in maps in the vector

Clara got `update-y` and `update-x` functions. The next step would be
to use these functions to update x and y values in maps in the vector.
This would not be a big deal for her anymore. Tricky thing was it
needed after updating y values in maps, it should update x values in
the same maps. Luckily, Clojure's `let` binding treats this sort of
value changes well. Within `let`, each binding is evaluated from the
first to last one by one. The `update` function turned to this:

```clojure
(defn update [state]
  (let [params  (:params state)
        params (map update-y params)
        params (map update-x params)]
    (assoc state :params params)))
```

The first binding assigns params vector to a name, `params`. The second
binding updates y values in the `params` (maps in vector) and assigns to the
name, `params`. The third binding updates x values in the `params`
(maps in vector) and assigns to the name, `params`. When `params`
comes to the body of `let` function, in other words, the line of
`assoc`, both x and y values are already updated.

These were all to swing the snowflakes. She could use `draw` function
as it was.


### `practice.clj` in step 7

At this point, her entire `practice.clj` looks like this:

```clojure
(ns drawing.practice
  (:require [quil.core :as q]
            [quil.middleware :as m]))

(defn setup []
  (q/smooth)
  {:flake (q/load-image "images/white_flake.png")
   :background (q/load-image "images/blue_background.png")
   :params [{:x 10  :swing 1 :y 10  :speed 1}       ;; swing was added in step 7-1
            {:x 200 :swing 3 :y 100 :speed 4}       ;;
            {:x 390 :swing 2 :y 50  :speed 2}]})    ;;

;; this update-x function was added in step 7-2
(defn update-x
  [m]
  (let [x (:x m)
        swing (:swing m)
        y (:y m)]
    (cond
     (< x 0) (assoc m :x (q/width))
     (< x (q/width)) (update-in m [:x] + (* swing (q/sin (/ y 50))))
     :else (assoc m :x 0))))

(defn update-y
  [m]
  (let [y (:y m)
        speed (:speed m)]
    (if (>= y (q/height))
      (assoc m :y 0)
      (update-in m [:y] + speed))))

(defn update [state]
  (let [params  (:params state)
        params (map update-y params)
        params (map update-x params)]               ;; added in step 7-3
    (assoc state :params params)))

(defn draw [state]
  (q/background-image (:background state))
  (let [params (:params state)]
    (dotimes [n 3]
      (let [param (nth params n)]
        (q/image (:flake state) (:x param) (:y param))))))

(q/defsketch practice
  :title "Clara's Quil practice"
  :size [500 500]
  :setup setup
  :update update
  :draw draw
  :features [:keep-on-top]
  :middleware [m/fun-mode])
```

When Clara ran this code, she saw snowflakes were falling down,
swinging left and right tracing sine curve.
"Cool!" she shouted with joy.

Still, she could a couple more improvements including refactoring,
Clara was satisfied with her app. Moreover, she started
thinking about her next, more advanced app in Clojure!

The End.


--------------
Snowflake is designed by Freepik, http://www.flaticon.com/packs/snowflakes
