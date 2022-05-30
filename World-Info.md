World information is stored as a nested dictionary. The top-level dictionary described below separates entities by type. Each entity sub-dictionary is described afterward.

| key | value type | value |
| --- | ---- | ----- |
| "tick" | int | Current simulation tick. |
| "score" | int | Current total score. |
| "actors" | dict | Actor information. [Actors](Actors)|
| "nodes" | dict | Node information. |
| "sites" | dict | Site information. |
| "buildings" | dict | Completed Building information. |
| "tasks" | dict | Task information. |
| "commands" | dict | Information on commands, both executing and completed. |

### Actors

| key | value type | value |
| --- | ---------- | ----- |
| actor_id (int) | dict | Actor dictionary |

**Actor Dictionary**
| key | value type | value |
| --- | ---------- | ----- |
| "id" | int | ID of the actor. |
| "node" | int | ID of the actor's current node. |
| "state" | int | Current state of the actor. <br />Actor.IDLE = 0<br />Actor.MOVING = 1<br />Actor.DIGGING = 2<br />Actor.CONSTRUCTING = 3<br />Actor.RECOVERING = 4<br />Actor.LOOKING = 5<br />Actor.SENDING = 6<br />Actor.RECEIVING = 7 |
| "progress" | int | Effort spent on current ongoing action. |
| "target" | int | ID of the target of the agent's current action. |
| "resources" | list | List of resource IDs held by the agent. |

(under construction)