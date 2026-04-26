[<< back](README.md)

# Screen wrap

> Enlace al artículo original:
> * https://kidscancode.org/godot_recipes/4.x/2d/screen_wrap/index.html

## Problema

Queremos que el jugador se pueda teletransportar de un lado al otro de la pantalla. Esta es una característica común, especialmente en los juegos 2D de la vieja escuela (por ejemplo Pac-man).

## Solución

1. Obtener el tamaño de la pantalla (viewport):

```
@onready var screen_size = get_viewport_rect().size
```

`get_viewport_rect()` es un método que está disponible en todos los nodos que heredan de `CanvasItem`.

2. Comparar la posición de tu jugador
```
    if position.x > screen_size.x:
        position.x = 0
    if position.x < 0:
        position.x = screen_size.x
    if position.y > screen_size.y:
        position.y = 0
    if position.y < 0:
        position.y = screen_size.y
```

El código anterior utiliza la posición del nodo, que generalmente es el centro de su objeto y/o cuerpo.

3. Simplificar con `wrapf()``

El código anterior se puede simplificar utilizando la función `wrapf()` de GDScript, que hace un "bucle" del valor entre los límites dados.
```
    position.x = wrapf(position.x, 0, screen_size.x)
    position.y = wrapf(position.y, 0, screen_size.y)
```
