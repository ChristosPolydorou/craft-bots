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

* `node_a`
  * One of the nodes that the edge connects to.
* `node_b`
  * One of the nodes that the edge connects to.
* `id`
  * This is the unique, positive, non-zero integer that represents the edge in the simulation.
* `length`
  * The length of the edge, which determines how long it takes for an actor to travel along the edge
* `get_other_node`
  * A function that when given the ID of a node, will return either: the ID of the other node the edge connects, if the ID of the node is one of the nodes the edge is connected to; None if the ID of the given node is not connected to the edge.
* `observers`*
  * This is a list of the `id`'s of the different actors that can see the edge.
  * _*This field is only available if edges are flagged as Partially Observable in the initialisation files (under rules)._

## Actors
Actors are the agent's method of acting on the CraftBots simulation. They can move between nodes, collect and move resources, construct buildings, make observations of the world, and communicate with other agents. An actor keeps track of its state, which determines what it is doing and what certain variables represent.

### Actor States
* `IDLE`
  * This is the state the actor has when it is not currently performing any actions.
  * `progress`: -1
  * `target`  : None
* `MOVING`
  * This is the state the actor has when it moving between two nodes along an edge.
  * `progress`: The distance the actor has travelled along the edge. Once this is equal to or greater than the length of the edge, the actor is set to `IDLE` and is considered to have arrived at the targetted node.
  * `target`  : (ID of the edge the actor is travelling along, ID of the Node the actor is travelling to)
* `DIGGING`
  * This is the state the actor has when it is digging at a mine to get resources. When the mine provides resources, then the actor will go `IDLE`.
  * `progress`: -1
  * `target`  : The ID of the mine the actor is digging at
* `CONSTRUCTING`
  * This is the state the actor has when it is constructing at a site or a green building. When the site or green building has reached the maximum possible progress, then the actor will go `IDLE`. 
  * `progress`: -1
  * `target`  : The ID of the Site / green building that the actor is constructing at
* `RECOVERING`
  * This is the state the actor has if it has failed a movement action (if Travel is Non-Deterministic). During this state, the actor will return to the node it started from, and cannot fail again. It will start from where it failed the action.
  * `progress`: The distance the actor has travelled along the edge. Once this is equal to or greater than the length of the edge, the actor is set to `IDLE` and is considered to have arrived at the targetted node.
  * `target`  : (ID of the edge the actor is travelling along, ID of the Node the actor is travelling to)
* `LOOKING`
  * This is the state the actor has when it is observing the world. In this state, the actor will be able to see further, allowing it to see partially observable objects outside the node the actor is currently at. The longer the actor is in the `LOOKING` state, the further it can see.
  * `progress`: The number of ticks the actor has been in the `LOOKING` state
  * `target`  : None
* `SENDING`
  * This is the state the actor has when it is broadcasting a message to actors that are `RECEIVING`.
  * `progress`: -1
  * `target`  : The message the actor is sending
* `RECEIVING`
  * This is the state the actor has when it is receiving messages broadcasted by actors that are `SENDING`.
  * `progress`: -1
  * `target`  : A list of tuples in the following format: (The tick the message was sent, the contents of the message)


### Fields
Actor entities have 6 fields that an agent can access to learn about the actor (7 if Actors are considered Partially Observable)

* `node`
  * The ID of the node the actor is currently at
* `state`
  * The current state of the actor. See above for the different states an actor can have and what they imply.
* `progress`
  * Depends on the `state` of the actor. See above for a list of states and what `progress` represents during that state.
* `id`
  * This is the unique, positive, non-zero integer that represents the actor in the simulation.
* `target`
  * Depends on the `state` of the actor. See above for a list of states and what `target` represents during that state.
* `resources`
  * A list of `id`'s of the resources the actor is currently holding
* `observers`*
  * This is a list of the `id`'s of the different actors that can see the actor.
  * _*This field is only available if actors are flagged as Partially Observable in the initialisation files (under rules)._

## Resources
Resources are what actors use to create buildings to complete tasks. Resources are created by actors mining at mines. There are 5 different types of resources, each with its own unique properties.

### Resource Types
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

### Fields
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

## Mines
Mines are stationary entities that exist at nodes and provide resources of the same colour as the mine. Mines and the process of digging at them may have special properties based on the colour of the mine. 

See resources properties to get an overview of what properties mines may have.

### Fields
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

## Sites
Sites are stationary entities that exist at nodes. They are created by actors at no cost but require resources and effort from the actors to construct. Actors can only put an amount of effort into constructing a site proportional to the number of resources deposited in the site to the total number of resources needed to build that site. 

Once the full amount of resources and effort have been put into a site, it is considered completed and turns into a building. 

### Fields
Site entities have 6 fields that an agent can access to learn about the site (7 if Sites are considered Partially Observable)

* `node`
  * The ID of the node the site is at
* `colour`
  * The colour of the Site (and the type of building it will create) (0 - red, 1 - blue, 2 - orange, 3 - black, 4 - green, 5 - purple)
* `deposited_resources`
  * A list of resources that have been deposited so far in the following format: [# of red resources, # of blue resources, # of orange resources, # of black resources, # of green resources]
* `needed_resources`
  * A list of the resources needed to complete the site in the following format: [# of red resources, # of blue resources, # of orange resources, # of black resources, # of green resources]
* `progress`
  * The amount of progress the site has towards being completed. It is increased by the actors constructing at the site and is limited by the number of deposited resources. The amount of progress depends on the number of resources needed to complete the building and the initialisation files.
* `id`
  * This is the unique, positive, non-zero integer that represents the building in the simulation.
* `observers`*
  * This is a list of the `id`'s of the different actors that can see the site.
  * _*This field is only available if sites are flagged as Partially Observable in the initialisation files (under rules)._

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

## Tasks
Tasks give the agent something to do in the Craftbots Simulation and act as a way to measure the performance of an agent. Tasks are randomly generated and at the start of each tick of the simulation, there is a chance for a new task to be generated. Each task will require the agent to have its actors build a purple building at a specific node. 

The amount of resources needed to do this is randomly generated and determines the number of points the agent gets for completing the task. There is also a chance (as determined by the initialisation files under rules) for a task to also have a deadline. If the building for the task is not completed before the deadline, then the agent will receive no points for completing the task.

### Fields
Task entities have 7 fields that an agent can access to learn about the task (8 if Tasks are considered Partially Observable)

* `node`
  * The ID of the node the task should be completed at
* `id`
  * This is the unique, positive, non-zero integer that represents the task in the simulation.
* `completed`
  * A function that when called will return True if the task is complete or the deadline has passed, and return False otherwise
* `difficulty`
  * The internally decided difficulty of the task (0 - easy, 1 - medium, 2 - hard)
* `needed_resources`
  * A list of the resources needed to build the tasks goal building in the following format: [# of red resources, # of blue resources, # of orange resources, # of black resources, # of green resources]
* `project`
  * The tasks assigned goal building or site. None if one does not exist.
* `deadline`
  * The tick that the deadline of the task is. If this is -1, then there is no deadline.
* `observers`*
  * This is a list of the `id`'s of the different actors that can see the task.
  * _*This field is only available if tasks are flagged as Partially Observable in the initialisation files (under rules)._

## Commands
Commands are instructions that the agent will send to actors via the API. The commands will be executed at the end of a tick. Each command will have a function given to it on the initialisation of the Command object. This determines the function the command will perform when it is set to perform. Commands also have a state that gives insight into what the progress of the Command currently is.

### Command States
* `PENDING`
  * The command is considered `PENDING` when the function it has been assigned to do has not been performed yet.
* `ACTIVE`
  * The command is considered `ACTIVE` when it is currently performing the function it has been assigned to do.
* `REJECTED`
  * The command is considered `REJECTED` when arguments give unexpected results, for example, giving an ID to a node when an ID to an actor is expected.
* `COMPLETED`
  * The command is considered `COMPLETED` when the function the command is assigned to do has been successfully completed, although the function itself might have failed, for other reasons, for example, if a pickup action by an actor fails, the Command would be considered complete, even if the action itself failed.

### Command Function ID's
* `MOVE_TO`
* `MOVE_RAND`
* `PICK_UP_RESOURCE`
* `DROP_RESOURCE`
* `DROP_ALL_RESOURCES`
* `DIG_AT`
* `START_SITE`
* `CONSTRUCT_AT`
* `DEPOSIT_RESOURCES`
* `CANCEL_ACTION`
* `START_LOOKING`
* `START_SENDING`
* `START_RECEIVING`

### Fields
Command entities have 5 fields that an agent can access to learn about the command. 

* `id`
  * This is the unique, positive, non-zero integer that represents the command in the simulation.
* `function_id`
  * The ID of the function that the Command will execute when performed. See above for a list of the function id's
* `args`
  * A list of the arguments for the function the command will perform
* `result`
  * The result of the function. This is None if the function has not yet been performed
* `state`
  * The state of the Command. See above for a list of states and their meaning.