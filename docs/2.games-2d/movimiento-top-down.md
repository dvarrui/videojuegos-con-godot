[<<back](README.md)

# Movimiento top-down

> Enlace al documento original: https://kidscancode.org/godot_recipes/4.x/2d/topdown_movement/index.html

**Problema**: controlar el movimiento de un personaje para un juego 2D top-down.

## Solución

Para esta solución, asumiremos que tenemos definidas las siguientes acciones:

| Nombre  | Tecla(s) |
| ------- | -------- |
| "up"    | W, ↑     |
| "down"  | S, ↓     |
| "right" | D, →     |
| "left"  | A, ←     |
| "clic"  | botón 1 del ratón |

También asumiremos que estamos usando un nodo `CharacterBody2D`.

Podemos resolver este problema de muchas maneras, dependiendo del tipo de comportamiento que estés buscando.

# Opción 1: movimiento en 8 direcciones

En este escenario, el jugador usa las cuatro teclas de dirección para moverse (incluidas las diagonales).

```python
extends CharacterBody2D

var speed = 400  # speed in pixels/sec

func _physics_process(delta):
    var direction = Input.get_vector("left", "right", "up", "down")
    velocity = direction * speed

    move_and_slide()
```

# Opción 2: rotar y mover

En este escenario, las acciones `left/right` rotan al personaje y `up/down` lo mueven hacia adelante y hacia atrás en cualquier dirección en la que mire. A esto a veces se le llama movimiento "estilo asteroides".

```python
extends CharacterBody2D

var speed = 400  # move speed in pixels/sec
var rotation_speed = 1.5  # turning speed in radians/sec

func _physics_process(delta):
    var move_input = Input.get_axis("down", "up")
    var rotation_direction = Input.get_axis("left", "right")
    velocity = transform.x * move_input * speed
    rotation += rotation_direction * rotation_speed * delta
    move_and_slide()
```

> **Nota**: En Godot un ángulo de 0 grados apunta a lo largo del eje x. Esto significa que la dirección de avance de un nodo (transform.x) es hacia la derecha. Debes asegurarte de que el objeto de tu personaje también esté dibujado apuntando hacia la derecha.

# Opción 3: apuntar con el mouse

Similar a la opción 2, pero esta vez la rotación del personaje se controla con el mouse (es decir, el personaje siempre apunta hacia el mouse). El movimiento hacia adelante/atrás se realiza con las teclas como antes.

```python
extends CharacterBody2D

var speed = 400  # move speed in pixels/sec

func _physics_process(delta):
    look_at(get_global_mouse_position())
    var move_input = Input.get_axis("down", "up")
    velocity = transform.x * move_input * speed
    move_and_slide()
```

# Opción 4: hacer clic y mover

En esta opción, el personaje se mueve a la ubicación en la que se hizo clic.

```python
extends CharacterBody2D

var speed = 400  # move speed in pixels/sec
var target = null

func _input(event):
    if event.is_action_pressed("click"):
        target = get_global_mouse_position()

func _physics_process(delta):
    if target:
        # look_at(target)
        velocity = position.direction_to(target) * speed
        if position.distance_to(target) < 10:
            velocity = Vector2.ZERO
    move_and_slide()
```

Hay que tenga en cuenta que dejamos de movernos si nos acercamos a la posición objetivo. Si no, el personaje "se moverá" hacia adelante y hacia atrás a medida que pasa un poco más allá del objetivo, retrocede, lo pasa un poco, y así sucesivamente. Opcionalmente, puedes usar look_at() para mirar en la dirección del movimiento.

[Descargar el código del proyecto](https://github.com/godotrecipes/topdown_movement)
