The `world_info` dictionary of the Agent class is updated by the simulation each tick. The top-level dictionary separates entities by type. Each entity sub-dictionary is described on this page.

| key         | value type | value |
| ----------- | ---------- | ----- |
| "tick"      | int        | Current simulation tick. |
| "score"     | int        | Current total score. |
| "actors"    | dict       | Actor information. [(jump)](world_info#actors) |
| "nodes"     | dict       | Node information. [(jump)](world_info#nodes)|
| "edges"     | dict       | Edge information. [(jump)](world_info#edges)|
| "mines"     | dict       | Mine information. [(jump)](world_info#mines)|
| "resources" | dict       | Resource information. [(jump)](world_info#resources)|
| "sites"     | dict       | Site information. [(jump)](world_info#sites)|
| "buildings" | dict       | Completed Building information. [(jump)](world_info#buildings)|
| "tasks"     | dict       | Task information. [(jump)](world_info#tasks)|
| "commands"  | dict       | Information on commands, both executing and completed. [(jump)](world_info#commands)|

## actors

| key | value type | value |
| --- | ---------- | ----- |
| actor_id (int) | dict | Actor dictionary |

**Actor Dictionary**
| key | value type | value |
| --- | ---------- | ----- |
| "id" | int | ID of the actor. |
| "node" | int | ID of the actor's current node. |
| "state" | int | Current state of the actor. |
| "progress" | int | Effort spent on current ongoing action. |
| "target" | int | ID of the target of the agent's current action. |
| "resources" | list | List of resource IDs held by the agent. |

**Actor States**
```python
Actor.IDLE         = 0
Actor.MOVING       = 1
Actor.DIGGING      = 2
Actor.CONSTRUCTING = 3
Actor.RECOVERING   = 4
Actor.LOOKING      = 5
Actor.SENDING      = 6
Actor.RECEIVING    = 7
```

## nodes

| key | value type | value |
| --- | ---------- | ----- |
| node_id (int) | dict | Node dictionary. |

**Node Dictionary**
| key | value type | value |
| --- | ---------- | ----- |
| "id" | int | ID of the node. |
| "actors"    | list | List of actor IDs currently at this node. |
| "tasks"     | list | List of task IDs that are located at this node. |
| "sites"     | list | List of site IDs started at this node. |
| "buildings" | list | List of completed building IDs at this node. |
| "resources" | list | List of resource IDs dropped at this node. |
| "mines"     | list | List of mine Ids at this node. |
| "edges"     | list | List of edges connected to this node. |
| "x"         | int  | Horizontal position of node. |
| "y"         | int  | Vertical position of node. |


## edges

| key | value type | value |
| --- | ---------- | ----- |
| edge_id (int) | dict | Edge dictionary. |

**Edge Dictionary**
| key | value type | value |
| --- | ---------- | ----- |
| "id"        | int | ID of the edge. |
| "length"    | int | Length of the edge. |
| "node_a"    | id | ID of one connected node. |
| "node_b"    | id | ID of the other connected node. |

## mines

| key | value type | value |
| --- | ---------- | ----- |
| mine_id (int) | dict | Mine dictionary. |

**Mine Dictionary**
| key | value type | value |
| --- | ---------- | ----- |
| "id"        | int | ID of the mine. |
| "node"      | int | ID of the node in which the mine is located. |
| "colour"    | int | Resource type of the mine (see resource). |
| "max_progress" | int | The total effort that must be made at this mine before it produces a resource. |
| "progress"     | int | The current effort expended at this mine. |

## resources

| key | value type | value |
| --- | ---------- | ----- |
| resource_id (int) | dict | Resource dictionary. |

**Resource Dictionary**
| key | value type | value |
| --- | ---------- | ----- |
| "id"           | int   | ID of the resource. |
| "colour"       | int   | Resource type. |
| "location"     | int   | ID of the resource's current location (node, actor, etc.) |
| "tick_created" | int   | The tick on which the resource was mined. |
| "used"         | bool  | True if the resource has been deposited into a site. |

**Resource Type**
```python
Resource.RED    = 0 # can only be collected within time windows
Resource.BLUE   = 1 # takes longer to collect
Resource.ORANGE = 2 # requires multiple actors to mine
Resource.BLACK  = 3 # cannot be carried with any other resource
Resource.GREEN  = 4 # decays over time
```

## sites

| key | value type | value |
| --- | ---------- | ----- |
| site_id (int) | dict | Site dictionary. |

**Site Dictionary**
| key | value type | value |
| --- | ---------- | ----- |
| "id"           | int   | ID of the site. |
| "building_type"| int   | Building type (see building). |
| "node"         | int   | ID of the node in which the site is located. |
| "deposited_resources" | list | List of length five describing the amount of each resource type deposited. |
| "needed_resources"    | list | List of length five describing the total amount of resource of each type required to construct the building. |
| "needed_effort"| int | The total effort that must be made at this site before it becomes a constructed building. |
| "max_progress" | int | The maximum effort that can be expended given the current number of deposited resource. |
| "progress"     | int | The current effort expended at this site. |

## buildings

| key | value type | value |
| --- | ---------- | ----- |
| building_id (int) | dict | Building dictionary. |

**Building Dictionary**
| key | value type | value |
| --- | ---------- | ----- |
| "id"            | int   | ID of the completed building. |
| "node"          | int   | ID of the node in which it was built. |
| "building_type" | int   | The type of completed building. |

**Building Type**
```python
Building.BUILDING_TASK         = 0 # scores points as dictated by a task
Building.BUILDING_SPEED        = 1 # increases movement speed of actors
Building.BUILDING_MINE         = 2 # increases mining rate of actors
Building.BUILDING_CONSTRUCTION = 3 # increases constructions rate of actors
Building.BUILDING_INVENTORY    = 4 # increases inventory capacity of actors
Building.BUILDING_ACTOR_SPAWN  = 5 # can be used to generate new actors
```

## tasks

| key | value type | value |
| --- | ---------- | ----- |
| task_id (int) | dict | Building dictionary. |

**Task Dictionary**
| key | value type | value |
| --- | ---------- | ----- |
| "id"            | int   | ID of the task. |
| "completed"     | bool  | ID of the node in which it was built. |
| "deadline"      | int   | The type of completed building. |
| "node"          | int   | The type of completed building. |
| "score"         | int   | The type of completed building. |
| "site"          | int   | The type of completed building. |
| "start_time"    | int   | The type of completed building. |
| "difficulty"    | int   | The type of completed building. |
| "needed_resources" | list | List of length five describing the required amount of resource of each type. |


## commands

| key | value type | value |
| --- | ---------- | ----- |
| command_id (int) | dict | Command dictionary. |

**Command Dictionary**
| key | value type | value |
| --- | ---------- | ----- |
| "id"           | int   | ID of the command. |
| "function_id"  | int   | The nature of the command. |
| "result"       | bool  | True if the command. |
| "state"        | int   | The current state of the command. |

**function_id**
```python
Command.MOVE_TO            = 0
Command.MOVE_RAND          = 1
Command.PICK_UP_RESOURCE   = 2
Command.DROP_RESOURCE      = 3
Command.DROP_ALL_RESOURCES = 4
Command.DIG_AT             = 5
Command.START_SITE         = 6
Command.CONSTRUCT_AT       = 7
Command.DEPOSIT_RESOURCES  = 8
Command.CANCEL_ACTION      = 9
Command.START_LOOKING      = 10
Command.START_SENDING      = 11
Command.START_RECEIVING    = 12
```

**Command State**
```python
Command.PENDING   = 0
Command.ACTIVE    = 1
Command.REJECTED  = 2
Command.COMPLETED = 3
```
