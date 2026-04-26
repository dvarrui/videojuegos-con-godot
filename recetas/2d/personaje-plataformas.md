[<< back](README.md)

# Personaje de plataformas

> Enlace al artículo original: https://kidscancode.org/godot_recipes/4.x/2d/platform_character/index.html

# Problema

Necesitamos crear un personaje estilo plataforma 2D.

# Solución

Los nuevos desarrolladores a menudo se sorprenden de lo complejo que puede ser programar un personaje de plataformas. Godot proporciona algunas herramientas integradas para ayudar, pero hay tantas soluciones como juegos. En este tutorial, no profundizaremos en funciones como saltos dobles, agacharse, saltos en la pared o animaciones. Aquí sólo veremos los fundamentos del movimiento de plataformas.

> **Consejo**
>
> Si bien es posible utilizar `RigidBody2D` para crear un personaje de plataformas, nos centraremos en `CharacterBody2D`. Los cuerpos cinemáticos son más adecuados para los juegos de plataformas, donde estamos menos interesado en la física realista y más en la sensación arcade.

Comizamos con un nodo `CharacterBody2D`. Le agregamos `Sprite2D` y `CollisionShape2D`.

Adjunte el siguiente script al nodo raíz del personaje. Tener en cuenta que estamos usando acciones de entrada que hemos definido en `InputMap`: `"walk_right"`, `"walk_left"` y `"jump"`.

```python
extends CharacterBody2D

@export var speed = 1200
@export var jump_speed = -1800
@export var gravity = 4000

func _physics_process(delta):
    # Add gravity every frame
    velocity.y += gravity

    # Input affects x axis only
    velocity.x = Input.get_axis("walk_left", "walk_right") * speed

    move_and_slide()

    # Only allow jumping when on the ground
    if Input.is_action_just_pressed("jump") and is_on_floor():
        velocity.y = jump_speed
```

Los valores utilizados para `speed`, `gravity` y `jump_speed` dependen en gran medida del tamaño del personaje. La textura del reproductor en este ejemplo es de 108x208 píxeles. Si tu objeto es más pequeño, querrás usar valores más pequeños. También queremos valores altos para que todo se sienta rápido y receptivo. Una gravedad baja da como resultado un juego con sensación de flotación, mientras que un valor alto significa que volverás rápidamente al suelo y estarás listo para saltar nuevamente.

Tenga en cuenta que estamos verificando `is_on_floor()` después de usar `move_and_slide()`. La función `move_and_slide()` establece el valor de este método, por lo que es importante no verificarlo antes, sino obtendremos el valor del frame anterior.
​
# Fricción y aceleración

El código anterior es un buen comienzo y se puede utilizar como base para una gran variedad de controladores de plataforma. Sin embargo, un problema que tiene es el movimiento instantáneo. Para una sensación más natural, es mejor si el personaje tiene que acelerar hasta su velocidad máxima y que se detenga cuando no hay ninguna acción.

Una forma de incluir este comportamiento es usar interpolación lineal (`lerp`). Al movernos, cambiaremos entre la velocidad actual y la velocidad máxima y mientras nos detenemos, cambiaremos entre la velocidad actual y 0. Ajustar la cantidad de `lerp` dará una variedad de diferentes estilos de movimiento.
​
> **Consejo**: Para obtener una descripción general de la interpolación lineal, consulte [Gamedev Math: Interpolación](https://kidscancode.org/godot_recipes/4.x/math/interpolation/).

```python
extends CharacterBody2D

@export var speed = 1200
@export var jump_speed = -1800
@export var gravity = 4000
@export_range(0.0, 1.0) var friction = 0.1
@export_range(0.0 , 1.0) var acceleration = 0.25


func _physics_process(delta):
    velocity.y += gravity
    var dir = Input.get_axis("walk_left", "walk_right")
    if dir != 0:
        velocity.x = lerp(velocity.x, dir * speed, acceleration * delta)
    else:
        velocity.x = lerp(velocity.x, 0.0, friction * delta)

    move_and_slide()
    if Input.is_action_just_pressed("jump") and is_on_floor():
        velocity.y = jump_speed
```

Pruebe a cambiar los valores `friction` y `acceleration` para ver cómo afectan a la sensación del juego. Un nivel de hielo, por ejemplo, podría tener valores muy bajos, lo que dificultaría las maniobras.

![](https://kidscancode.org/godot_recipes/4.x/img/platformer1.gif)

# Conclusión

Este código le ofrece un punto de partida para crear su propio controlador de plataformas. Para funciones de plataformas más avanzadas, como saltos de pared, consulte las otras recetas.

[Descargar este proyecto](https://github.com/godotrecipes/2d_platform_basic)
