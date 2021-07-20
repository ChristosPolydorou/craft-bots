# Mines
Mines are stationary entities that exist at nodes and provide resources of the same colour as the mine. Mines and the process of digging at them may have special properties based on the colour of the mine. 

See resources properties to get an overview of what properties mines may have.

## Fields
Mine entities have 4 fields that an agent can access to learn about the mine (5 if Mines are considered Partially Observable)

* `node`
  * The ID of the node the Mine is at
* `colour`
  * The colour of the mine (and the type of resource it would provide) (0 - red, 1 - blue, 2 - orange, 3 - black, 4 - green)
* `id`
  * This is the unique, positive, non-zero integer that represents the site in the simulation.
* `progress`
  * The progress towards the mine providing a new resource. The effort the progress must reach is determined by the initialisation files.
* `observers`*
  * This is a list of the `id`'s of the different actors that can see the mine.
  * _*This field is only available if mine are flagged as Partially Observable in the initialisation files (under rules)._
