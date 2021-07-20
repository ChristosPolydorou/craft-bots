# Actors
Actors are the agent's method of acting on the CraftBots simulation. They can move between nodes, collect and move resources, construct buildings, make observations of the world, and communicate with other agents. An actor keeps track of its state, which determines what it is doing and what certain variables represent.

## Actor States
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


## Fields
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