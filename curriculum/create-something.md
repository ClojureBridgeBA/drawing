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

;; setup and draw functions and q/defsketch are added in step 1-3
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
            [quil.middleware :as m]))  ;; this line is added in step 1-5

(defn setup [])

(defn draw [state])                    ;; argument state is added in step 1-5

(q/defsketch practice
  :title "Clara's Quil practice"
  :size [500 500]
  :setup setup
  :draw draw
  :features [:keep-on-top]
  :middleware [m/fun-mode])             ;; this line is added in step 1-5
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
  ;; these two lines, a map (data structure) is added in step 1-6
  {:flake (q/load-image "images/white_flake.png")
   :background (q/load-image "images/blue_background.png")}
  )

(defn draw [state]
  ;; q/background-image and q/image functions are added in step 1-6
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
      ;; updating y paraemter by one
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
  ;; drawing blue background and a snowflake on it
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
  (q/smooth)                                      ;; added in step 2-2
  {:flake (q/load-image "images/white_flake.png")
   :background (q/load-image "images/blue_background.png")
   :y-param 10}                                   ;; added in step 2-1
  )

;; update function is added in step 2-1
(defn update [state]
  (update-in state [:y-param] inc))

(defn draw [state]
  (q/background-image (:background state))
  (q/image (:flake state) 200 (:y-param state))    ;; changed in step 2-2
  )

(q/defsketch practice
  :title "Clara's Quil practice"
  :size [500 500]
  :setup setup
  :update update                                   ;; added in step 2-1
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

## Step 4. Make more snowflakes falling down from top to bottom

Clara thought, "It's nice to look at the snowflake falls down many
times. But I have only one snowflake. Can I add more?"
She wanted to see more snowflakes falling down: one or more on the left
half, as well as one or more on the right half.

Again, she needed to express her thoughts by the words of programming
world. Looking at her code already written, she figures out, "This
would be 'draw multiple images with the different `x` parameters.'"
The easiest way would be to copy-paste `(q/image (:flake state) 200 (:y-param state)`
multiple times with the different `x` parameters. For example,

```clojure
(q/image (:flake state) 10 (:y-param state))
(q/image (:flake state) 200 (:y-param state))
(q/image (:flake state) 390 (:y-param state))
```

But, for Clara, this did not look nice; she learned a lot about
Clojure and wanted to use what she knew.

First, she thought about how to keep multiple `x` parameters. She
remembered there was a `Vectors` in the
[Data Structures](http://clojurebridge.github.io/curriculum/outline/data_structures.html)
section, which looked a good fit in this case.

Here's what she did to add more snowflakes:

1. Add a vector which has multiple `x` parameters assigned to a name
with `def`.

    ```clojure
    (def x-params [10 200 390]) ;; x parameters for three snowflakes
    ```

2. Draw snowflakes as many times as the number of `x`-params using `doseq`.

    ```clojure
    (doseq [x x-params]
      (q/image (:flake state) x (:y-param state)))
    ```

* See, [doseq](http://clojurebridge.github.io/curriculum/outline/sequences.html#/3)

### `practice.clj` in step 4

At this point, `practice.clj` looks like this:

```clojure
(ns drawing.practice
  (:require [quil.core :as q]
            [quil.middleware :as m]))

(def x-params [10 200 390])                      ;; added in step 4

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
  (doseq [x x-params]                              ;; two lines were changed
    (q/image (:flake state) x (:y-param state))))  ;; in step 4

(q/defsketch practice
  :title "Clara's Quil practice"
  :size [500 500]
  :setup setup
  :update update
  :draw draw
  :features [:keep-on-top]
  :middleware [m/fun-mode])
```

"Yes!" Clara shouted when she saw three snowflakes kept falling down.


## Step 5. Make snowflakes keep falling down at different speed

Although her app looked lovely, Clara felt something was not quite
right. In her window, all three snowflakes fell down at the same
speed like a robots' march. It did not look natural. So, she wanted to
make them fall down at different speed.

Using programming terms, the problem here is that all three snowflakes
share the same `y` parameter.
Given that using multiple `y` parameters would solve the problem--but how?

### step 5-1 Change `y` parameters to maps with speeds

As she used `vector` for the x parameters, the `vector` would be a good data
structure to have different `y` parameters as well. However, this
should not be a simple vector since each snowflake will have two
parameters, height and speed. Having these two, Clara could change the
falling speed of each snowflake.

Well, she was back to ClojureBridge curriculum and went to
[Data Structures](http://clojurebridge.github.io/curriculum/outline/data_structures.html).
There was a data structure called `Maps` which allowed her to save
multiple parameters. Looking at maps examples, she changed the `y-param` from a
single value to a vector of 3 maps. Also, the keyword was changed from
`y-param` to `y-params`. Now her initial **state** became this:

```clojure
{:flake (q/load-image "images/white_flake.png")
 :background (q/load-image "images/blue_background.png")
 :y-params [{:y 10 :speed 1} {:y 150 :speed 4} {:y 50 :speed 2}]}
```

It was a nice data structure, actually maps in a vector in a map
including outer map. Downside was, her `update` function would not be
simple anymore. What she had to do was updating all `y` values in
the three maps within a vector.


### step 5-2 Update values in maps in the vector

Thinking both map and vector at the same time was confusing to her, so
she decided to think about map only. Each map has `y` parameter and
`speed`:

```clojure
{:y 150 :speed 4}
```
as an initial **state**. The very next moment, speed should be added
to y value. As a result, the map should be updated to:

```clojure
{:y 154 :speed 4}
```

To accomplish this map update, Clara added `update-y` function:

```clojure
(defn update-y
  [m]
  (let [y (:y m)
        speed (:speed m)]
    (if (>= y (q/height))           ;; y is greater than or equal to image height?
      (assoc m :y 0)                ;; true - get it back to the 0 (top)
      (update-in m [:y] + speed)))) ;; false - add y value and speed
```

#### [bonus] destructuring

Using Clojure's destructuring, we can write `update-y` function like
this:

```clojure
(defn update-y
  [{y :y speed :speed :as m}]
  (if (>= y (q/height))
    (assoc m :y 0)
    (update-in m [:y] + speed)))
```

As in the code, we can skip let binding.


### step 5-3 Update maps in the vector

Next step is to update maps in the vector using `update-y` function.
Before writing this part, Clara cut down the problem to focus on
updating a vector: how to update contents in a vector. She remembered
there was a `map` function which allowed her to apply a function to
each element in the vector.
[`map` function](http://clojurebridge.github.io/curriculum/outline/functions.html#/9)

For example:

```clojure
(map inc [1 2 3]) ;=> (2 3 4)
```

> Clojure has a `map` function and `map` data structure.
> Be careful, this is confusing.
> In Python, function is a `map`, data structure is dictionary.
> In Ruby, function is a `map` or `collect`, data structure is hash.

She tested the function on the insta-REPL:

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

It looked good, so she got back to her `practice.clj` file to change
`update` function. This function should return the **state** as the
map which includes `:y-params` key with the update vector as a value.
Her `update` function became like this:

```clojure
(defn update [state]
  (let [y-params (:y-params state)]
    (assoc state :y-params (map update-y y-params))))
```


### step 5-4 Update draw function to see maps in the vector

Another challenge was the `draw` function change to see values in the
maps which were in the vector.
Clara found a couple of ways to repeat something in Clojure. Among
them, she chose `dotimes` and `nth` to repeatedly draw images; the
`nth` function is the one in the curriculum:
[Data Structures](http://clojurebridge.github.io/curriculum/outline/data_structures.html#/6)

In this case, she knew there were exactly 3 snowflakes, so she changed
the code to draw 3 snowflakes as shown below:

```clojure
(let [y-params (:y-params state)]
    (dotimes [n 3]
      (q/image (:flake state) (nth x-params n) (:y (nth y-params n)))))
```

### `practice.clj` in step 5

At this point, her entire `practice.clj` looks like this:

```clojure
(ns drawing.practice
  (:require [quil.core :as q]
            [quil.middleware :as m]))

(def x-params [10 200 390])

(defn setup []
  (q/smooth)
  {:flake (q/load-image "images/white_flake.png")
   :background (q/load-image "images/blue_background.png")
   :y-params [{:y 10 :speed 1} {:y 150 :speed 4} {:y 50 :speed 2}]})  ;; changed in step 5-1

;; update-y function was added in step 5-2
(defn update-y
  [m]
  (let [y (:y m)
        speed (:speed m)]
    (if (>= y (q/height))
      (assoc m :y 0)
      (update-in m [:y] + speed))))

(defn update [state]
  (let [y-params (:y-params state)]                   ;; update function
    (assoc state :y-params (map update-y y-params)))) ;; was changed in step 5-3

(defn draw [state]
  (q/background-image (:background state))
  (let [y-params (:y-params state)]   ;; three lines below were changed in step 5-4
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

When she ran the code, three snowflakes kept falling down at
different speeds. It looked more natural.


## Step 6. Do some "refactoring"

Clara looked at her `practice.clj` thinking her code got longer for a while.

Scanning her code from top to bottom again, she thought
"`x-params` may be part of the **state**," for example:

```clojure
[{:x 10 :y 10 :speed 1} {:x 200 :y 150 :speed 4} {:x 390 :y 50 :speed 2}]
```

She found that this new data structure was easy to maintain the state of each snowflake.

To use this new data structure, she changed her `setup` function to
return the initial **state** which included x parameters also. The key
name was changed from `:y-params` to `:params`:

```clojure
{:flake (q/load-image "images/white_flake.png")
   :background (q/load-image "images/blue_background.png")
   :params [{:x 10  :y 10  :speed 1}
            {:x 200 :y 150 :speed 4}
            {:x 390 :y 50  :speed 2}]}
```

Clara stared at `update-y` function and concluded to leave as it was.
Since existence of `:x` and its value didn't affect updating y value.
The function returned the map with three key-value pairs with updated y value.

What about `update` function? This needed a little change since key
name was changed from `:y-params` to `:params`.

```clojure
(defn update [state]
  (let [params (:params state)]
    (assoc state :params (map update-y params))))
```

The `draw` function would have a bigger change since the way to
extract x values was changed.
At first, Clara changed `dotimes` function like this:

```clojure
(let [params (:params state)]
  (dotimes [n 3]
    (q/image (:flake state) (:x (nth params n)) (:y (nth params n)))))
```

But, the exact the same thing, `(nth params n)`, appeared twice.
"Is there anything better to avoid repetition?" she thought.
The answer was easy - use `let` binding within `dotimes` function.
Using `let`, her `dotimes` form turned to:

```clojure
(let [params (:params state)]
  (dotimes [n 3]
    (let [param (nth params n)]
      (q/image (:flake state) (:x param) (:y param)))))
```

The last line got much cleaner!

### `practice.clj` in step 6

At this moment, her entire `practice.clj` looks like this:

```clojure
(ns drawing.practice
  (:require [quil.core :as q]
            [quil.middleware :as m]))

(defn setup []
  (q/smooth)
  {:flake (q/load-image "images/white_flake.png")
   :background (q/load-image "images/blue_background.png")
   :params [{:x 10  :y 10  :speed 1}                       ;; changed in step 6
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
  (let [params (:params state)]                    ;; changed to params in step 6
    (assoc state :params (map update-y params))))  ;;

(defn draw [state]
  (q/background-image (:background state))
  (let [params (:params state)]                            ;; changed in step 6
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

She saw the exact same result as the step 5, but her code
looked nicer. This sort of work is often called "refactoring".

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
