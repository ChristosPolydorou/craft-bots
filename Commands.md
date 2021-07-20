## Commands
Commands are instructions that the agent will send to actors via the API. The commands will be executed at the end of a tick. Each command will have a function given to it on the initialisation of the Command object. This determines the function the command will perform when it is set to perform. Commands also have a state that gives insight into what the progress of the Command currently is.

### Command States
* `PENDING`
  * The command is considered `PENDING` when the function it has been assigned to do has not been performed yet.
* `ACTIVE`
  * The command is considered `ACTIVE` when it is currently performing the function it has been assigned to do.
* `REJECTED`
  * The command is considered `REJECTED` when arguments give unexpected results, for example, giving an ID to a node when an ID to an actor is expected.
* `COMPLETED`
  * The command is considered `COMPLETED` when the function the command is assigned to do has been successfully completed, although the function itself might have failed, for other reasons, for example, if a pickup action by an actor fails, the Command would be considered complete, even if the action itself failed.

### Command Function ID's
* `MOVE_TO`
* `MOVE_RAND`
* `PICK_UP_RESOURCE`
* `DROP_RESOURCE`
* `DROP_ALL_RESOURCES`
* `DIG_AT`
* `START_SITE`
* `CONSTRUCT_AT`
* `DEPOSIT_RESOURCES`
* `CANCEL_ACTION`
* `START_LOOKING`
* `START_SENDING`
* `START_RECEIVING`

### Fields
Command entities have 5 fields that an agent can access to learn about the command. 

* `id`
  * This is the unique, positive, non-zero integer that represents the command in the simulation.
* `function_id`
  * The ID of the function that the Command will execute when performed. See above for a list of the function id's
* `args`
  * A list of the arguments for the function the command will perform
* `result`
  * The result of the function. This is None if the function has not yet been performed
* `state`
  * The state of the Command. See above for a list of states and their meaning.