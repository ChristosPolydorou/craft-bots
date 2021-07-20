# Edges
Edges act as a connection between two nodes, allowing actors to travel between them. Together with nodes, they create a graph that represents the world of the Craftbots Simulation. 

## Fields
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