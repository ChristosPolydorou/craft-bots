# Nodes
Nodes represent locations in the Craftbots world. They can contain actors, resources, mines, sites, and buildings. Nodes can also have certain tasks assigned to them. Nodes are connected to one another via edges. Nearly all actions that actors can take will be performed in nodes. 

## Fields
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
