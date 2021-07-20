# Resources
Resources are what actors use to create buildings to complete tasks. Resources are created by actors mining at mines. There are 5 different types of resources, each with its own unique properties.

## Resource Types
* Red
  * Red resources can only be gathered during certain intervals, as specified by the initialisation files
* Blue
  * Blue resources require extra effort to be gathered, as specified by a multiplier in the initialisation files
* Orange
  * Orange resource requires two or more actors to be actively mining at an orange mine to be created. 
* Black
  * Black resources take up the entire "inventory space" of the actor that is holding it, meaning that only one black resource can be held at a time, and no other resources can be held at the same time as a black resource. Similarly, an actor cannot pick up a black resource if it is holding anything else.
  * To help ease the process of managing and interacting with black resources, the API provides a command to have an actor drop everything in its inventory into the node it is currently at.
* Green 
  * Green resources will decay over time and will eventually disappear, as determined by the initialisation files.

## Fields
Resource entities have 5 fields that an agent can access to learn about the resource (6 if Resources are considered Partially Observable)

* `id`
  * This is the unique, positive, non-zero integer that represents the resource in the simulation.
* `location`
  * The ID of the actor or node that is currently holding the resource
* `tick_created`
  * The tick that the resource was created
* `used`
  * True if the resource has been deposited, or decayed, False otherwise
* `colour`
  * The colour of the resource (0 - red, 1 - blue, 2 - orange, 3 - black, 4 - green)
* `observers`*
  * This is a list of the `id`'s of the different actors that can see the resource.
  * _*This field is only available if resources are flagged as Partially Observable in the initialisation files (under rules)._