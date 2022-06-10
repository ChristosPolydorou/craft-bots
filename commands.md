Commands are sent by the agent via the API to control actors and are executed at the end the tick on which they were sent. Each command is given a unique ID and can be observed like other entities in the simulation. See [world_info#command](world_info#command).

## Dispatching

## Monitoring

| PENDING   | 0
| ACTIVE    | 1
| REJECTED  | 2
| COMPLETED | 3

## Detailed Command List

- The *expected duration* of an action is the actual duration in ticks, unless temporal uncertainty is enabled.

| Command name | function_id | arguments | expected duration | description |
| ------------ | ----------- | --------- | ----------------- | ----------- |
| MOVE_TO            | 0  | actor_id, node_id | Determined by actor move speed and edge length. | The actor will travel from its current node to the connected node specified by node_id. |
| MOVE_RAND          | 1  | actor_id | Determined by actor move speed and edge length. | The actor will travel to a random connected node. |
| PICK_UP_RESOURCE   | 2  | actor_id, resource_id | The specified resource will be moved from the node to the actor's inventory. |
| DROP_RESOURCE      | 3  | actor_id, resource_id | 1 tick | | 
| DROP_ALL_RESOURCES | 4  | actor_id | 1 tick | |
| DIG_AT             | 5  | actor_id, mine_id | Determined by the mine's max progress and actor's mining rate. | |
| START_SITE         | 6  | actor_id, [task_id] | 1 tick | |
| CONSTRUCT_AT       | 7  | actor_id, site_id | Determined by the site's max progress and actor's construction rate. | |
| DEPOSIT_RESOURCES  | 8  | actor_id, site_id, resource_id | 1 tick | |
| CANCEL_ACTION      | 9  | actor_id | 1 tick | |
| START_LOOKING      | 10 | actor_id | | |
| START_SENDING      | 11 | actor_id, message | | |
| START_RECEIVING    | 12 | actor_id | | |


