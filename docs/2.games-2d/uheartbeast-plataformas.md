[<< back](README.md)

# Juego de plataformas por uheartbeast

Creación de un juego de plataformas por **uheartbeast**.


## Vídeo 1

> [Godot 4 Tutorial - Heart Platformer](https://youtu.be/M8-JVjtJlIQ?si=Z7OCVC6I2mOnybkh)

* Elige el modo "compatibilidad" porque va a crear un proyecto pequeño.
* Escena World

```python
extends Node2D

@onready var collision_polygon_2d = $StaticBody2D/CollisionPolygon2D
@onready var polygon_2d = $StaticBody2D/CollisionPolygon2D/Polygon2D

func _ready():
  RenderingServer.set_default_clear_color(Color.BLACK)
  polygon_2d.polygon = collision_polygon_2d.polygon
```
* Player GDScript

```python
extends CharacterBody2D

const SPEED = 100.0
const ACCELERATION = 800.0
const FRICTION = 1000.0
const JUMP_VELOCITY = -300.0

var gravity = ProjectSettings.get_settings("physics/2d/default_gravity")
@onready var animated_sprite_2d = $AnimatedSprite2D

func _physics_process(delta):
  apply_gravity(delta)
  handle_jump()
  var input_axis = Input.get_axis("ui_left", "ui_right")
  handle_acceleration(input_axis, delta)
  apply_friction(input_axis, delta)
  update_animations(input_axis)
  move_and_slide()

func apply_gravity(delta):
  if not is_on_floor():
    velocity.y += gravity * delta

func handle_jump():
  if is_on_floor():
    if Input.is_action_just_pressed("ui_up"):
      velocity.y = JUMP_VELOCITY
    else:
      if Input.is_action_just_released("ui_up") and velocity.y < JUMP_VELOCITY/2:
        velocity.y = JUMP_VELOCITY / 2

func handle_acceleration(input_axis, delta):
  if input_axis != 0:
    velocity.x = move_towards(velocity.x, SPEED * input_axis, ACCELERATION * delta)

func apply_friction(input_axis, delta):
  if input_axis == 0:
    velocity.x = move_towards(velocity.x, 0, FRICTION * delta)

func update_animations(input_axis):
  if input_axis != 0:
    animated_sprite_2d.flip_h = (input_axis < 0)
    animated_sprite_2d.play("run")
  else:
    animated_sprite_2d.play("idle")

  if not is_on_floor():
    animated_sprite_2d.play("jump")
```

* Se añade un nodo AnimatedSprite2D para las animaciones del Player.

## Vídeo 2 - Coyote jump

> [Godot 4 Tutorial - Heart Platformer P2 - Coyote Jump](https://youtu.be/U0aMEdlJ4Io?si=jvwCOmAd6omkMyhr)

El "Coyote jump" se puede implementar de forma sencilla con un Timer.

## Vídeo 3 - Custom Resources

> [Godot 4 Tutorial - Heart Platformer P3 - Player Movement Data (Custom Resources)](https://youtu.be/kh1U1Xg_VrA?si=YRkKzuwnOsWWn1_N)

## Vídeo 4 - Double jump and wall jump

> [Dodot 4 Tutorial - Heart Platformer P4 - Double Jump and Wall Jump](https://youtu.be/z9CRmVSS5mg?si=AO9NVoSdnOtrtJEl)

## Vídeo 5

> [Godot 4 Tutorial - Heart Platformer P5 - Hazards and Collision Layers/Masks](https://youtu.be/4nlyb2DZzUo?si=XaRVhCD6UtbIqS9j)

* Configura las capas de la física 2D de modo que únicamente el player detecta las colisiones con el mundo y los peligros.
* Los peligros son Area2D sin las colisiones. Debemos añadir las colisiones cada vez que queramos usar el nodo HazardArea dentro de un peligro concreto (Spike por ejemplo).

## Vídeo 6

> [Godot 4 Tutorial - Heart Platfomer P6 - Collectables + Level Completed](https://youtu.be/4dy1kFUv7mA?si=w-B1pTTjGkydl7qG)

* El nodo `CenterContainer` debe estar bajo un nodo `Node` para que se posicione correctamente, pero aún es mejor que sea hijo de un nodo `CanvasLayer` para que la posición se actualice con el movimiento de la cámara.
* Los coleccionables activarán "Level completed" cuando ya no quede ninguno.

```python
func _on_body_entered(body):
  queue_free()
  var hearts = get_tree().get_nodes_in_group("Hearts")
  if hearts.size == 1:
    Events.level_completed.emit()
```

* Crear un autoload llamado `Events` y dentro se define la señal `level_completed`.

```python
extends Node

signal level_completed
```

* World se conecta para reaccionar a los eventos de `Events.level_completed.connect(show_level_completed)`.
* Los coleccionables pueden emitir la señal `Events.level_completed.emit()`


## Vídeo 7

> [Godot 4 Tutorial - Heart Platformer P7 - Autotile + Sloped Tiles](https://youtu.be/MsXB2mK23Ng?si=UNvxnuBQhokLn-oX)

* Los tiles que el terreno deje mal se pueden arreglas pintando tiles individuales encima de las celdas adecuadas.

## Vídeo 8

> [Godot 4 Tutorial - Heart Platformer P8 - Level Transitions and Inherited Scenes](https://youtu.be/vh-sx5Y32kE?si=6MLhOgc81WrPjcfg)

## Vídeo 9

> [Godot 4 Tutorial - Heart Platformer P9 - Controller Support](https://youtu.be/9-GJJDf_gTY?si=N5maZe27nCzmyx1K)
