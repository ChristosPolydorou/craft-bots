## Contents

- [Sending-Commands](#Sending-Commands)
- [Querying](#Querying)

## Sending Commands

Commands are sent by the agent via the API to control actors and are executed at the end the tick on which they were sent. There exists a seperate function for each possible command. The functions are listed below; the commands are described in detail on the [commands](33_commands#Detailed-Command-List) page.

```python
    def move_to(self, actor_id, node_id)
    def move_rand(self, actor_id)
    def pick_up_resource(self, actor_id, resource_id)
    def drop_resource(self, actor_id, resource_id)
    def drop_all_resources(self, actor_id)
    def dig_at(self, actor_id, mine_id)
    def start_site(self, actor_id, task_id=None)
    def construct_at(self, actor_id, site_id)
    def deposit_resources(self, actor_id, site_id, resource_id)
    def start_looking(self, actor_id)
    def cancel_action(self, actor_id)
    def start_sending(self, actor_id, message)
    def start_receiving(self, actor_id)
    def get_world_info(self)
    def get_by_id(self, entity_id, entity_type=None, target_node=None)
    def get_field(self, entity_id, field, entity_type=None, target_node=None)
```

## Querying

World information can be accessed in two ways.
1. Through the nested [world_info](32_world_info) dictionary that is updated each tick, or
2. using the `get_field` function.

Description of get_field:
```python
    def get_field(self, entity_id, field, entity_type=None, target_node=None):
        """
        Returns the specified field of an entity.

        Optionally, the type of the entity can be passed in as a string to stop the function from searching other types
        of entities and the ID of the node that should be searched can also be chosen to only search one node for the
        specified entity.

        :param entity_id: The ID of the entity that should be found
        :param field: The field of the entity that should be returned
        :param entity_type: (optional) The type of entity to be found (Node, Edge, Actor, Resource, Mine, Site, Building)
        :param target_node: (optional) the ID of the node that should be checked
        :return: The field from the entity, or None if the entity is not found or the entity does not have the field.
        """
```

Example of using the `get_field` function:
```python
# using the world info dictionary to interate through tasks
for task_id, task in self.world_info['tasks'].items(): 

    # get the ID of the site, if it is started, otherwise None.
    site_id = self.api.get_field(task_id, "site")

    # get the ID of the task's node.
    target_node = self.api.get_field(task_id, "node")
```

