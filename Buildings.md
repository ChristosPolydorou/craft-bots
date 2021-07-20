# Buildings
Buildings are stationary entities that exist at nodes. They can either provide positive bonuses to the actors or points to the agent, depending on the colour of the building. Buildings are created by completing a site, at which point the site will turn into a building of the same colour at the node it is at. 

Certain Buildings are linked to tasks and if completed before the deadline, if one exists, then the agent is given a certain amount of points, depending on the difficulty of the task.

## Building Types
There are 6 different buildings that can be constructed that each provides a different bonus:
* Red buildings
  * Red buildings provide a percentage increase to the speed at which actors can move between nodes.
* Blue buildings
  * Blue buildings provide a percentage increase to the speed at which actors can dig at mines.
* Orange buildings
  * Orange buildings provide a percentage increase to the speed at which actors can construct at sites.
* Black buildings
  * Black buildings provide a flat increase to the inventory size of actors, increasing the number of resources an actor can hold in one go.
  * _This does not affect the property of black resources._
* Green buildings
  * Green buildings allow actors to deposit resources and construct at the buildings, similar to the functionality of Sites. The outcome of completing construction at the green building yields a new actor.
* Purple buildings
  * Purple buildings are assigned to tasks. A purple site can only be built at a node that has a task that doesn't already have a site or building assigned to it. The amount of resources needed to build a purple building is random each time and determines the number of points the agent will get upon completion of the building. 

## Fields
Building entities have 3 fields that an agent can access to learn about the building. If the building is green, then there are 6 fields ( + 1 if Buildings are considered Partially Observable)

* `node`
  * The ID of the node the building is at
* `colour`
  * The colour of the building (0 - red, 1 - blue, 2 - orange, 3 - black, 4 - green, 5 - purple)
* `id`
  * This is the unique, positive, non-zero integer that represents the building in the simulation.
* `needed_resources`*
  * The number of resources needed to complete the construction of a new actor in the following format: [# of red resources, # of blue resources, # of orange resources, # of black resources, # of green resources]
  * _*This field is only available if the building is green._
* `deposited_resources`*
  * The number of resources deposited into the building in the following format: [# of red resources, # of blue resources, # of orange resources, # of black resources, # of green resources]
  * _*This field is only available if the building is green._
* `progress`*
  * The amount of progress the site has towards being completed. It is increased by the actors constructing at the site and is limited by the number of deposited resources. The amount of progress depends on the number of resources needed to complete the building and the initialisation files.
  * _*This field is only available if the building is green._
* `observers`*
  * This is a list of the `id`'s of the different actors that can see the building.
  * _*This field is only available if buildings are flagged as Partially Observable in the initialisation files (under rules)._
