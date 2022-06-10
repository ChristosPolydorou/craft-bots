# Actors
When the simulation starts, the AI will have 3 actors given to them. These actors are controlled by the AI agent you are tasked with designing. They can be controlled to move to different nodes, perform different tasks, such as digging and building, and pick up and put down resources, of which they can carry only a limited amount.

* MOVE - Move between two nodes along an existing connection.
* DIG - Produce one resource at a node that contains a mine of the corresponding type.
* PICKUP - Pick up one resource.
* DROP - Drop one resource at the current node.
* CREATE SITE - Creates a site used to construct a building at the current node
* CONSTRUCT - Build a structure at the current node, up to the percentage of resources that has been deposited into the site.
* DEPOSIT - Deposit resources into a site, to allow the actor to continue constructing the buildings


# Resources
There are 5 different resources that can spawn and be used to construct buildings. These are: Red, Orange, Blue, Black, and Green. Red resources can only be gathered during certain intervals of the simulation. Orange resources require two agents to be mined. Blue resources take considerably longer to gather than the other resources. Black resources take up the entire inventory of the actor, as such, any actor can only carry one Black resource at a time. Green resources decay over time, and will eventually disappear, if not used.
Resources each take a certain amount of time to mine. Once mined, they are dropped onto the floor. Resources on the floor can be picked up to be moved somewhere else, and resources in an actor’s inventory can be dropped to make space for different resources, or to store them there.


# Buildings
There are 5* different buildings available. The buildings are: Battery Facilities, Management Buildings, Tool Sheds, Cotton Mills, and Bot Factories. Battery Facilities increase the speed at which actors can move between nodes. Management Buildings increase how fast actors can construct buildings. Tool sheds increase the rate at which resources are mined by actors. Cotton Mills increases the inventory size of actors; however, this does not affect the Black resource’s property of taking the entire inventory space of an actor. Bot Factories, when fully built, allow actors to bring resources into the building to create more actors. This gives the benefit of giving more actors for your AI to use, with the downside of having to manage more actors.
Each building requires a different amount of some/all of the different resources. Buildings take time to build, and can only be built to a percentage equal to the percentage of resources currently given to the building site. Once a resource is placed in a building site it is consumed. Resources can also be placed in the same node as a building site and not be consumed. There can also be more than one building/ building site at one node. Buildings that provide an effect to the actors do so globally, affecting all actors in the simulation.
