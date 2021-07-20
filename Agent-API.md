The Agent API is how agents communicate with the simulation. When `AgentAPI` is created the simulation will give it a list of Actor ID's that it can interact with. This is stored in AgentAPI under `actors`. The agent that is given this API can only see through and command the actors in that list. The following commands are available to the agent via the API:

* `move_to(actor_id, node_id)`
  * Tell an actor to begin moving to the given node.
  * To do this, the actor must be idle, and the target node must be adjacent to the node the actor is currently at.
  * Parameters
    * actor_id: The ID of the actor to be moved
    * node_id: The ID of the node the actor should move to.
  * returns: The ID of the command or -1 if the max command limit has been reached or API does not have access to the actor

* `move_rand(actor_id)`
  * Tell the actor to move to a randomly chosen adjacent node.
  * To do this, the actor must be idle.
  * Parameters
    * actor_id: The ID of the actor to be moved
  * returns: The ID of the command or -1 if the max command limit has been reached or API does not have access to the actor

* `pick_up_resource(actor_id, resource_id)`
  * Tell an actor to pick a specific resource off of the ground.
  * To do this, the actor must be idle, the resource must be in the same node as the actor, the resource must not be held by another actor, and the actor should have space in its inventory to store the resource. If the resource is black then the actor should not be holding any other resources. The actor should also not be holding a black resource already.
  * Parameters
    * actor_id: The ID of the actor to pick up the resource
    * resource_id: The ID of the resource to be picked up
  * returns: The ID of the command or -1 if the max command limit has been reached or API does not have access to the actor

* `drop_resource(actor_id, resource_id)`
  * Tell an actor to drop a specific resource from its inventory. The resource will be dropped at the node the actor is at.
  * To do this, the actor must be idle and the resource must be in the actor's inventory.
  * Parameters
    * actor_id: The ID of the actor to drop the resource
    * resource_id: The ID of the resource to be dropped
  * returns: The ID of the command or -1 if the max command limit has been reached or API does not have access to the actor

* `drop_all_resources(actor_id)`
  * Tell an actor to drop all of the resources it is currently holding. This will drop all of the resources it is currently holding into the node the actor is currently at.
  * To do this, the actor must be idle.
  * Parameters
    * actor_id: The ID of the actor to drop all of its resources
  * returns: The ID of the command or -1 if the max command limit has been reached or API does not have access to the actor

* `dig_at(actor_id, mine_id)`
  * Tell an actor to begin digging at a mine. Assuming the special mining conditions of the mine's resource are met, after a certain amount of time, a new resource will be placed on the node the mine and actor are at and then the actor will stop digging.
  * To do this, the actor must be idle, and the actor should be at the same node as the mine.
  * Parameters
    * actor_id: The ID of the actor to dig at the mine
    * mine_id: The ID of the mine the actor should dig at
  * returns: The ID of the command or -1 if the max command limit has been reached or API does not have access to the actor

* `start_site(actor_id, site_type)`
  * Tell an actor to start a site of the specified type. A site will be placed on the node that the actor is at.
  * To do this the actor must be idle. If the site is purple, there must be a task that does not have a site or building assigned to it at the node the actor is at.
  * Parameters
    * actor_id: The ID of the actor to create a site
    * site_type: The type of site to be built (0: red, 1: blue, 2: orange, 3: black, 4: green, 5: purple)
  * returns: The ID of the command or -1 if the max command limit has been reached or API does not have access to the actor

* `construct_at(actor_id, site_id)`
  * Tell the actor to begin constructing at the specified site. The actor will construct up to a percentage equal to the deposited resources / needed resources at the site. If the actor completes construction then the site will become a building at the same node, and the actor will become idle. Actors can also construct at green buildings in the same manner in order to create new actors.
  * To do this, the actor must be idle and the actor must be at the same node as the site.
  * Parameters
    * actor_id: The ID of the actor to construct at the site
    * site_id: The ID of the site/building the actor should construct at.
  * returns: The ID of the command or -1 if the max command limit has been reached or API does not have access to the actor

* `deposit_resources(actor_id, site_id, resource_id)`
  * Tell the actor to deposit a resource into the site. This will increase the maximum progress that can be done on the site. Actors can also do this on green buildings to commit resources towards creating new actors.
  * To do this, the actor must be at the same node as the site, the resource must be in the actor's inventory, and the site should still need the type of the resource.
  * Parameters
    * actor_id: The ID of the actor to deposit the resource at the site
    * site_id: The ID of the site the resources should be deposited in
    * resource_id: The ID of the resource to be deposited
  * returns: The ID of the command or -1 if the max command limit has been reached or API does not have access to the actor

* `start_looking(actor_id)`
  * Tells the actor to begin "looking" this action only has an effect when the simulation is partially observable. Assuming that the simulation is partially observable when an actor is doing anything (aside from moving) it can see everything that is currently at the node it is at, but not further. If the actor is moving, then it can only see itself. However, if the actor is looking then it will stay in place and do nothing. During this time, it will look further every few ticks. After each interval of ticks is required to see deeper that passes since the actor has started looking, the actor can see everything an extra step further. Any information gathered this way is provided in world_info.
  * To do this, the actor must be idle.
  * Parameters
    * actor_id: The ID of the actor that should begin looking
  * returns: The ID of the command or -1 if the max command limit has been reached or API does not have access to the actor

* `cancel_action(actor_id)`
  * Tells the actor to stop any ongoing action it is currently performing. Depending on what the actor is currently doing depends on what the actor does. If the actor is moving between two nodes, then the actor will turn around attempt to return to the node it started at. If the actor is digging, then it will become idle, and if no other actors are digging at the node, then the progress towards a new resource is lost. If the actor is constructing, then the actor becomes idle, and the progress in the site stops at the amount it is at.
  * To do this, the actor must be moving, digging, constructing, looking, sending, or receiving
  * Parameters
    * actor_id: The ID of the actor to cancel its action
  * returns: The ID of the command or -1 if the max command limit has been reached or API does not have access to the actor

* `start_sending(actor_id, message)`
  * Tells the actor to start sending a message. This is intended to be used during a simulation with limited communications. The message can be whatever the agent decides. Only actors that are actively receiving can hear a message, and only when the two actors are at the same node.
  * To do this, the actor must be idle.
  * Parameters
    * actor_id: The ID of the actor to start sending a message
    * message: The message the agent wishes to broadcast.
  * returns: The ID of the command or -1 if the max command limit has been reached or API does not have access to the actor

* `start_receiving(actor_id)`
  * Tells an actor to listen for a message being broadcasted by an actor that is sending. The two actors must be in the same node for the message to be transmitted. When the actor receives a message, it will store it in a list with any other messages it has received, as well as the tick the message was received, under the actor's target.
  * To do this, the actor must be idle.
  * Parameters
    * actor_id: The ID of the actor to start receiving messages
  * returns: The ID of the command or -1 if the max command limit has been reached or API does not have access to the actor

* `get_world_info()`
  *This gets an up to date version of the world_info dictionary instantly. This is to be used if your agent takes a long time to deliberate actions or is waiting for something to change, and the agent needs up to date information immediately. This does not need to be called at the start of get_next_commands because the world info is updated each time before the function is called.
  * returns: The world_info dictionary

* `get_by_id(entity_id, entity_type=None, target_node=None)`
  * This command instantly returns the fields of an entity. The fields are stored in a dictionary and are updated every tick.
  * Optionally, the type of the entity can be passed in as a string to stop the function from searching other types of entities and the ID of the node that should be searched can also be chosen to only search one node for the specified entity.
  * Parameters
    * entity_id: The ID of the entity that should be found
    * entity_type: (optional) The type of entity to be found (Node, Edge, Actor, Resource, Mine, Site, Building)
    * target_node: (optional) the ID of the node that should be checked
  * returns: A dictionary of the fields the entity has, or None if the entity is not found

* `get_field(entity_id, field, entity_type=None, target_node=None)`
  * This command instantly returns the specified field of an entity. The field should be stored in the fields of the entity.
  * Optionally, the type of the entity can be passed in as a string to stop the function from searching other types of entities and the ID of the node that should be searched can also be chosen to only search one node for the specified entity.
  * Parameters
     * entity_id: The ID of the entity that should be found
     * field: The field of the entity that should be returned
     * entity_type: (optional) The type of entity to be found (Node, Edge, Actor, Resource, Mine, Site, Building)
     * target_node: (optional) the ID of the node that should be checked
  * returns: The field from the entity, or None if the entity is not found or the entity does not have the field.