## Contents

- [Dispatching](#Dispatching)
- [Monitoring](#Monitoring)
- [Detailed Command List](#Detailed-Command-List)

## Dispatching

Commands are sent by the agent via the API to control actors and are executed at the end the tick on which they were sent. Each command is given a unique ID. Commands are dispatched through the [agent API](../blob/main/api/agent_api.py) by calling the appropriate function. For example, the following code dispatches the command for an actor to travel to another node. The ID of the command is saved for monitoring.
```python
command_id = self.api.move_to(actor_id, node_id)
```

If the arguments are malformed, then the function will return `-1`. Please check the following causes:
- One of the argument IDs does not exist.
- The argument IDs reference the wrong kind of object (i.e. the value of `node_id` refers to another actor, not a node).
- The maximum number of commands has already been sent this tick. Note that this should be much higher than the number of actors.

## Monitoring

Commands can be monitored through the [world_info](32_world_info#command) dictionary or by using the `get_field` function. The "state" field contains the execution information available to the command, which is updated each tick.

| Command State | value | description |
| ------------- | ----- | ----------- |
| Command.PENDING    | 0 | The command has been sent, but not yet received by the actor. |
| Command.ACTIVE     | 1 | The command has been recieved by the actor and is currently executing. |
| Command.REJECTED   | 2 | The command was rejected by the actor. |
| Command.PREEMPTING | 3 | A cancellation command has been received and the actor is finishing executing. |
| Command.ABORTED    | 4 | The action has been aborted. |
| Command.SUCCEEDED  | 5 | The action has been successfully completed. |
| Command.PREEMPTED  | 6 | The action was cancelled by another command. |

For example, the following code requests the information for a move command that was already dispatched.
```python
command_info = self.api.get_field(command_id, "state")
print("State:", str(command_info))
```
and produces the following output:
```
State: 2
```
Indicating that the action is currently executing.

## Detailed Command List

- The *expected duration* of an action is the actual duration in ticks, unless temporal uncertainty is enabled.

| Command name | function_id | arguments | expected duration | description |
| ------------ | ----------- | --------- | ----------------- | ----------- |
| MOVE_TO            | 0  | actor_id, node_id | Determined by actor move speed and edge length. | The actor will travel from its current node to the connected node specified by node_id. |
| MOVE_RAND          | 1  | actor_id | Determined by actor move speed and edge length. | The actor will travel to a random connected node. |
| PICK_UP_RESOURCE   | 2  | actor_id, resource_id | The specified resource will be moved from the node to the actor's inventory. |
| DROP_RESOURCE      | 3  | actor_id, resource_id | 1 tick | The specified resource will be moved from the actor's inventory to the node. | 
| DROP_ALL_RESOURCES | 4  | actor_id | 1 tick | All resources in the actor's inventory will be moved into the node. |
| DIG_AT             | 5  | actor_id, mine_id | Determined by the mine's max progress and actor's mining rate. | The actor will begin digging at the mine until either the action is preempted, fails, of the mine's max effort is reached and a resource is produced. |
| START_SITE         | 6  | actor_id, task_id | 1 tick | Creates a new site at the node into which resources can be deposited. The site is linked to the specified task and will score that task once the building is completed. |
| CONSTRUCT_AT       | 7  | actor_id, site_id | Determined by the site's max progress and actor's construction rate. | The actor will begin constructing at the site until either the action is preempted, fails, of the site's max progress is reached. |
| DEPOSIT_RESOURCES  | 8  | actor_id, site_id, resource_id | 1 tick | The resource is removed from the actor's inventory and added to the site's deposited resources. |
| CANCEL_ACTION      | 9  | actor_id | 1 tick | Preempts the actor's current action. |
| START_LOOKING      | 10 | actor_id | TBC | TBC |
| START_SENDING      | 11 | actor_id, message | TBC | TBC |
| START_RECEIVING    | 12 | actor_id | TBC | TBC |


