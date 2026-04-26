[<< back](README.md)

# Autotiles con Tilemap y TileSet

Con los nodos TileMap y TileSet se construyen "mundos" a base de celdas o tiles.
* El `TileSet` es el conjunto de celdas disponibles y
* `TileMap` representa una disposición concreta de las celdas para conformar un "mundo" o entorno o escena de juego.

A la hora de rellenar el `TileMap` escogemos cada tipo de celta del `TileSet` y las iremos colocando en el mapa donde queramos. Diseñamos el mapa usando tiles individuales. Cada celda la tenemos que pintas en el mapa manualmente.

También podemos diseñar el mapa usando `Terrains` (autotiles) que puede ser más rápido en determinados casos. Los `Terrains` son conjuntos de tiles enlazados de tal forma que Godot puede "adivinar" cómo colocarlos. Con los terrenos pintamos una celda y automáticamente se nos crean los diferentes tiles.

> Enlace de interés:
> * [Vídeo - Godot 4 Autotile Tutorial](https://youtu.be/TvCqukvUu6U?si=MS9FMPTYmu8VSnis)
> * [How to Use Tile Bitmasking to Auto-Tile Your Level Layouts](https://code.tutsplus.com/how-to-use-tile-bitmasking-to-auto-tile-your-level-layouts--cms-25673t)

## Proceso

* Empezamos añadiendo un nodo del tipo `TileMap`.
* Seleccionamos el nodo `TileMap` y creamos un nuevo `TileSet`.
* Seleccionamos el TileSet en el inspector. Agregamos una imagen png con los tiles en la pestaña de Tiles.
* Inspector -> TileSet -> Terrain Set para crear un nuevo terreno.
* Podemos usar 3 tipos de sistemas de autotile (terrenos):
    * Esquinas y lados (3x3)
    * Esquinas (2x2)
    * Lados
* Creamos un terreno nuevo. Le damos un nombre y un color.
* En la sección "Paint" elegimos la propiedad "Terrain". Seleccionamos TerrainSet/Terrain y definimos el bitmask.
  * Podemos descargar a modo de ejemplo una [plantilla](https://github.com/FHaisal/youtube-resources/tree/main/Simple%20Autotile) para crear el bitmask correspondiente a cada tipo de terreno.
* Para probarlo vamos al TileMap -> Terrains y dibujamos el mapa en el editor.
