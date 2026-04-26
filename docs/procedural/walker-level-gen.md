[<< back](README.md)

# Walker level gen by GDQuest

> Enlaces al vídeo  original:
> * [Random Level Generation with a Walker - Godot 3 Tutorial 1](https://youtu.be/2nk6bJBTtlA?si=PODKKqsA-gTe5uPR)
    * [Github code](https://github.com/uheartbeast/walker-level-gen)

`Walker level gen` es un generador de niveles 2D usando la técnica del caminante o `walker`. La forma de generar el nivel es hacer que un caminante se mueva de forma aleatoria y vaya dejando huecos a medida que avanza.

El código tiene las siguientes escenas: World y Walker.
* World: es la escena principal que muestra el laberinto.
* Walker: es la clase encargada de crear un nuevo laberinto.

Pasos que ejecuta Walker para crear un level:
* Partimos de un mapa 2D representado por una rejilla o matriz completamente lleno de obstáculos. A continuación de se elige un punto inicial dentro de la rejilla y un número de pasos para el "caminante".
* El caminante se desplaza un paso y hace un hueco en la rejilla.
* De forma aleatoria se decide avanzar o cambiar de dirección (Cuando la probabilidad es menor 0.25 y se han dado al menos 4 pasos)
* El proceso se repite hasta completar un número de pasos máximo.

> Enlaces al vídeo  original:
> * [Rooms in a Walker Level Generator - Godot 3 Tutorial 2](https://youtu.be/1O83wdW5hAs?si=sSb45erzop_FoLZD)

En este segundo vídeo se modifica el algoritmo del caminante para generar un nivel con espacios con forma de habitaciones.
* Se produce un cambio de dirección cuando se han recorrido al menos 6 pasos.
* En el momento que se produce el cambio de dirección de fuerza la creación de un espacio "habitación" de tamaño aleatorio entre 2 y 5.

> Enlaces al vídeo  original:
> * [Random Level Generation (walker) with a Player and an Exit - Godot 3 tutorial 3](https://youtu.be/K7sKUuSd8ME?si=8N2XRUg5WX7GTWb1)

En este tercer vídeo se añaden 2 escenas: Player y Exit.
* Player: es un KinematicBody2D que podremos desplazar por los huecos del laberinto.
* Exit: es una Area2D que dispara la señal para crear un nuevo level cuando el Player colisiona con ella.
