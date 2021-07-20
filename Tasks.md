# Tasks
Tasks give the agent something to do in the Craftbots Simulation and act as a way to measure the performance of an agent. Tasks are randomly generated and at the start of each tick of the simulation, there is a chance for a new task to be generated. Each task will require the agent to have its actors build a purple building at a specific node. 

The amount of resources needed to do this is randomly generated and determines the number of points the agent gets for completing the task. There is also a chance (as determined by the initialisation files under rules) for a task to also have a deadline. If the building for the task is not completed before the deadline, then the agent will receive no points for completing the task.

## Fields
Task entities have 7 fields that an agent can access to learn about the task (8 if Tasks are considered Partially Observable)

* `node`
  * The ID of the node the task should be completed at
* `id`
  * This is the unique, positive, non-zero integer that represents the task in the simulation.
* `completed`
  * A function that when called will return True if the task is complete or the deadline has passed, and return False otherwise
* `difficulty`
  * The internally decided difficulty of the task (0 - easy, 1 - medium, 2 - hard)
* `needed_resources`
  * A list of the resources needed to build the tasks goal building in the following format: [# of red resources, # of blue resources, # of orange resources, # of black resources, # of green resources]
* `project`
  * The tasks assigned goal building or site. None if one does not exist.
* `deadline`
  * The tick that the deadline of the task is. If this is -1, then there is no deadline.
* `observers`*
  * This is a list of the `id`'s of the different actors that can see the task.
  * _*This field is only available if tasks are flagged as Partially Observable in the initialisation files (under rules)._