World information is stored as a nested dictionary. The top-level dictionary described below separates entities by type. Each entity sub-dictionary is described afterward.

| key         | value type | value |
| ----------- | ---------- | ----- |
| "tick"      | int        | Current simulation tick. |
| "score"     | int        | Current total score. |
| "actors"    | dict       | Actor information. [(jump)](world_info#actors) |
| "nodes"     | dict       | Node information. [(jump)](world_info#nodes)|
| "resources" | dict       | Resource information. [(jump)](world_info#resources)|
| "sites"     | dict       | Site information. [(jump)](world_info#sites)|
| "buildings" | dict       | Completed Building information. [(jump)](world_info#buildings)|
| "tasks"     | dict       | Task information. [(jump)](world_info#tasks)|
| "commands"  | dict       | Information on commands, both executing and completed. [(jump)](world_info#commands)|

### actors

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

### nodes

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

### resources

| key | value type | value |
| --- | ---------- | ----- |
| resource_id (int) | dict | Resource dictionary. |

**Resource Dictionary**
| key | value type | value |
| --- | ---------- | ----- |
| "id"           | int   | ID of the resource. |
| "location"     | int   | ID of the resource's current location (node, actor, etc.) |
| "tick_created" | int   | The tick on which thr resource was mined. |
| "used"         | bool  | True if the resource has been deposited into a site. |

### buildings

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
Building.BUILDING_TASK         = 0
Building.BUILDING_SPEED        = 1
Building.BUILDING_MINE         = 2
Building.BUILDING_CONSTRUCTION = 3
Building.BUILDING_INVENTORY    = 4
Building.BUILDING_ACTOR_SPAWN  = 5
```

### commands

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

### edges

2: {'get_other_node': <bound method Edge.get_other_node_id of Edge(Node(1), Node(0), 86)>,
               'id': 2,
               'length': 86,
               'node_a': 1,
               'node_b': 0},

### mines
26: {'colour': 0,
                'id': 26,
                'max_progress': 100,
                'node': 1,
                'progress': 0},
### sites

49: {'building_type': 0,
                'deposited_resources': [0, 0, 0, 0, 0],
                'id': 49,
                'max_progress': 0.0,
                'needed_effort': 100,
                'needed_resources': [0, 0, 0, 0, 1],
                'node': 6,
                'progress': 0}},

### tasks
': {21: {'completed': <bound method Task.completed of Task(Node(17), [0, 0, 0, 0, 1])>,
                'deadline': -1,
                'difficulty': 0,
                'id': 21,
                'needed_resources': [0, 0, 0, 0, 1],
                'node': 17,
                'score': 2,
                'site': None,
                'start_time': 0},

