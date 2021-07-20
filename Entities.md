# Entities
The CraftBots simulation has a collection of different entities that represent the Craftbots world, and allow your agent to communicate and understand it. There are 8 different entities in the Craftbots simulation:
* Nodes
* Edges
* Actors
* Resources
* Mines
* Sites
* Buildings
* Tasks

Each entity, among its properties and a unique ID provided by the simulation, also has a set of fields that an agent can read, stored in a dictionary. Each entity is described in more detail below. Commands sent by the agent to the simulation are also treated as entities, and information about the state of a command can be read in a similar fashion to other entities, via the fields.

## Nodes
Nodes represent locations in the Craftbots world. They can contain actors, resources, mines, sites, and buildings. Nodes can also have certain tasks assigned to them. Nodes are connected to one another via edges. Nearly all actions that actors can take will be performed in nodes. 

### Fields
Nodes have 10 fields that an agent can access to learn about a node (11 if Nodes are considered Partially Observable).
* `x`
  * This is the node's x coordinate in the simulation. In conjunction with the y coordinate, these can be used to find exact distances between different nodes, if your agent should require that.
* `y`
  * This is the node's y coordinate in the simulation. See `x`.
* `edges`
  * This is a list of the `id`'s of the different edges that connect to the node.
* `actors`
  * This is a list of the `id`'s of the different actors that are currently at the node. This also lists actors that are currently leaving the node via one of the edges.
* `resources`
  * This is a list of the `id`'s of the different resources that are on the ground at the node. This does not include resources being held by actors in the node or resources deposited inside sites / green buildings.
* `mines`
  * This is a list of the `id`'s of the different mines that are at the node.
* `sites`
  * This is a list of the `id`'s of the different sites that are at the node.
* `buildings`
  * This is a list of the `id`'s of the different buildings that are at the node.
* `tasks`
  * This is a list of the `id`'s of the different tasks that require something to be built at the node.
* `id`
  * This is the unique, positive, non-zero integer that represents the node in the simulation.
* `observers`*
  * This is a list of the `id`'s of the different actors that can see the node.
  * _*This field is only available if nodes are flagged as Partially Observable in the initialisation files (under rules)._

## Edges
Edges act as a connection between two nodes, allowing actors to travel between them. Together with nodes, they create a graph that represents the world of the Craftbots Simulation. 

### Fields
Edge entities have 5 fields that an agent can access to learn about the edge (6 if Edges are considered Partially Observable)

* ``
  *
* ``
  *
* ``
  *
* ``
  *
* ``
  *
* ``
  *

## Actors
Actors are the agent's method of acting on the CraftBots simulation. They can move between nodes, collect and move resources, construct buildings, make observations of the world, and communicate with other agents.

### Fields
Actor entities have 6 fields that an agent can access to learn about the actor (7 if Actors are considered Partially Observable)

* ``
  *
* ``
  *
* ``
  *
* ``
  *
* ``
  *
* ``
  *
* ``
  *

## Resources
Resources are what actors use to create buildings to complete tasks. Resources are created by actors mining at mines. There are 5 different types of resources, each with its own unique properties.

### Resource Types
* Red
  * Red resources can only be gathered during certain intervals, as specified by the initialisation files
* Blue
  * Blue resources require extra effort to be gathered, as specified by a multiplier in the initialisation files
* Orange
  * Orange resource require two or more actors to be actively mining at an orange mine to be created. 
* Black
  * Black resources take up the entire "inventory space" of the actor that is holding it, meaning that only one black resource can be held at a time, and no other resources can be held at the same time as a black resource. Similarly, an actor cannot pick up a black resource if it is holding anything else.
  * To help ease the process of managing and interacting with black resources, the API provides a command to have an actor drop everything in its inventory into the node it is currently at.
* Green 
  * Green resources will decay over time and will eventually disappear, as determined by the initialisation files.

### Fields
Resource entities have 5 fields that an agent can access to learn about the resource (6 if Resources are considered Partially Observable)

* ``
  *
* ``
  *
* ``
  *
* ``
  *
* ``
  *
* ``
  *

## Mines
Mines are stationary entities that exist at nodes and provide resources of the same colour as the mine. Mines and the process of digging at them may have special properties based on the colour of the mine. 

See resources properties to get an overview of what properties mines may have.

### Fields
Mine entities have 5 fields that an agent can access to learn about the mine (6 if Mines are considered Partially Observable)

* ``
  *
* ``
  *
* ``
  *
* ``
  *
* ``
  *
* ``
  *

## Sites
Sites are stationary entities that exist at nodes. They are created by actors at no cost but require resources and effort from the actors to construct. Actors can only put an amount of effort into constructing a site proportional to the number of resources deposited in the site to the total number of resources needed to build that site. 

Once the full amount of resources and effort have been put into a site, it is considered completed and turns into a building. 

### Fields
Site entities have 5 fields that an agent can access to learn about the site (6 if Sites are considered Partially Observable)

* ``
  *
* ``
  *
* ``
  *
## Buildings
Buildings are stationary entities that exist at nodes. They can either provide positive bonuses to the actors or points to the agent, depending on the colour of the building. Buildings are created by completing a site, at which point the site will turn into a building of the same colour at the node it is at. 

Certain Buildings are linked to tasks and if completed before the deadline, if one exists, then the agent is given a certain amount of points, depending on the difficulty of the task.

### Building Types
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

### Fields
Building entities have 5 fields that an agent can access to learn about the building (6 if Buildings are considered Partially Observable)

## Tasks
Tasks give the agent something to do in the Craftbots Simulation and act as a way to measure the performance of an agent. Tasks are randomly generated and at the start of each tick of the simulation, there is a chance for a new task to be generated. Each task will require the agent to have its actors build a purple building at a specific node. 

The amount of resources needed to do this is randomly generated and determines the number of points the agent gets for completing the task. There is also a chance (as determined by the initialisation files under rules) for a task to also have a deadline. If the building for the task is not completed before the deadline, then the agent will receive no points for completing the task.

### Fields
Task entities have 5 fields that an agent can access to learn about the task (6 if Tasks are considered Partially Observable)

## Commands
Commands are instructions that the agent will send to actors via the API. The commands will be executed at the end of a tick. 

### Fields
Command entities have 5 fields that an agent can access to learn about the command