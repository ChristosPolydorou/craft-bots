![craftbots screenshot](https://raw.githubusercontent.com/strathclyde-artificial-intelligence/craft-bots/main/screenshot.png)

Craftbots is a simple simulation with multiple actors that is loosely inspired by the scenario of the Robocup logistics league. There are many customisation options that can be used to emulate different scenarios. This page describes the basic rules as configured in the [simulation_configuration.yaml](../blob/main/craftbots/config/simulation_configuration.yaml). The [configuration](20_configuration) page contains detailed information on how the scenario can be customised.

**Basic Scenario**
- The simulation consists of a team of *actors*, located at *nodes*, and the goal is to complete *tasks* by constructing *buildings*. 
- The simulation has a set horizon within which the actors must complete the tasks.
- Actors construct buildings by starting *sites*, depositing *resources* into those sites, and then spending time constructing.
- Resources are collected from *mines*. They come in five resource types (colours): red, blue, green, orange, black. 

**Actors**
- Actors are controlled by *agents*. See [creating an agent](30_creating_an_agent).
- Actors can move from node to node, collect resources from mines, pick up and drop resources at nodes, deposit resources into sites, and construct sites. They can perform observation and communication actions if those are enabled in the configuration.
- Actors can only hold three resources at one time.
- Actions are described on the [commands](33_commands#detailed-command-list) page.

**Tasks**
- A task looks like "construct a building at node X using these resources: 2 red, 1 blue, ..."
- Each task is associated with a score and a deadline. Constructing the required building before the deadline will complete the task and score the points.
- When all tasks are completed, new tasks are generated.
- There is a small chance that a new task is generated each tick.

**Mines**
- Mines are located at nodes.
- Each mine has a specific resource type.
- When actors dig at a mine they contribute thier effort to a single progress value. When the effort reaches max progress a single resource is spawned in the node. *Note: Actors digging in parallel produce one resource twice as quickly.*
- When the actors stop digging at the mine, the progress made so far is not lost.

**Resources**
- Resources can be carried by actors, dropped, picked up, and deposited into sites. Once a resource has been deposited into a site, it cannot be retrieved.
- Each resource type has a unique property:
  - *black* cannot be carried with any other resource. Picking up a black resource while the inventory is not empty will fail. Similarly attempting to pick up any resource while already carrying one black resource will fail.
  - *red* resource can only be collected within time windows. Interaction with red mines outside of the time window is not possible.
  - *blue* resource takes must longer to produce from a mine.
  - *orange* resource requires multiple actors to collect. Digging at an orange mine with only a single actor does not increase its progress.
  - *green* resources decay over time. If they are not deposited within a set period after they are mined, they disappear.
- resource properties can be enabled/disabled and configured in the [configuration](20_configuration).

**Sites and Buildings**
- Sites are started at nodes and linked to a task.
- Actors deposit resources into a site.
- When actors construct at a site they contribute their effort into a single progress value. When the effort reaches the "needed effort" then the building is constructed.
- Progress cannot be increased beyond the "max progress" which is a fraction or the needed effort equal to the fraction of required resources so far deposited. For example, if the task requires two resources and only one has been deposited, then the max progress is 50% of the needed effort.