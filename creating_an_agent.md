## Contents

- [Overview](#Overview)
- [Base Agent](#Base-Agent)
- [Example](#Example)

## Overview

In Craftbots **Agents** control one or more **actors** by sending commands. [Commands](commands) are dispatched through the [agent API](../blob/main/api/agent_api.py) by calling the appropriate function. Information about the current visible state of the world can be read from a [world information dictionary](world_info). For convenience the world info dictionary can be queried through the API.

To adapt an existing planning and execution system to the simuation:
1. Create an adapter that inherits the (Base Agent)[Base-Agent] class.
2. connect the adapter to the simulation by appending them to the simulation's `agents` list. See [main.py](../blob/main/main.py) for an example. 

!(adapter image)[../blob/gh-pages/craftbots_adapter.png]

*Note: the examples below assume an agent intended to run in its own thread. If lockstep is enabled in the configuration then the get_next_commands will be called every tick, and the simulation will be blocked until it returns.*

## Base Agent

The [base agent class](../blob/main/agents/agent.py) is shown below.
```python
class Agent:

    def __init__(self):

        # simulation status flags
        self.simulation_complete = False
        self.simulation_paused = False

        # simulation interface
        self.api : agent_api.AgentAPI = None
        self.world_info : dict = None

        # agent status flag
        self.thinking = False            

    def get_next_commands(self):
        pass
```
The simulation interface and status flags are set by the simuation.
- While `simulation_complete` remains false, the simuation tick limit has not been reached.
- While `simulation_paused` is true, the world is not updated. This occurs when the user pauses the sim through the GUI.
- `self.api` provides a handle to the agent interface through which commands can be sent and world information queried.
- `self.world_info` contains all of the world information as a dictionary and is updated each tick. See [world_info](world_info).

The `self.thinking` agent status flag should be set by both agent and simulation. Each tick the simuation will check the flag, and if false will set the flag to true and call the agent's `get_next_commands` function. If the agent wishes the simulation to call the method multiple times, this can be done by resetting the flag to false.

## Example

The Python code for the test agent is shown below. This agent moves actors randomly. A more detailed explanation follows.
```python
from agents.agent import Agent
from craftbots.entities.actor import Actor
from craftbots.log_manager import Logger

class TestAgent(Agent):

    def __init__(self):
        super().__init__()            

    def get_next_commands(self):

        Logger.info("Agent", "Starting random moves.")

        while not self.simulation_complete:
            for actor_id in self.api.actors:
                if self.api.get_field(actor_id, "state") == Actor.IDLE:
                    self.api.move_rand(actor_id)

        Logger.info("Agent", "Finished.")
```

The test agent is a subclass of the base agent class.
```python
class TestAgent(Agent):
    
    def __init__(self):
        super().__init__()  
```

When the simulation begins the `get_next_commands` method is called at the beginning of the first tick. The `thinking` flag is never set to false, so the method will not be called again.
```python
    def get_next_commands(self):
```

The agent makes use of the in-built craftbots [logger](../blob/main/craft_bots/log_manager.py). The logger can be used to collate information and error messages on screen, in file, and in the GUI.
```python
        Logger.info("Agent", "Starting random moves.")
```

The agent loops until the simulation is finished. 
```python
        while not self.simulation_complete:
```

Each agent is checked to see if it is in an idle state. The `self.api.actors` list stores actor IDs for convenience - but these can also be found in the usual way through the world information (see here)[world_info#actors]. The actor state is queried using the `get_field` function of the agent api.
```python
            for actor_id in self.api.actors:
                if self.api.get_field(actor_id, "state") == Actor.IDLE:
```
For more information on actor fields, see (world_info#actors)[world_info#actors].

Those actors that are idle are moved randomly by sending a [command](commands) through the agent api.
```python
                    self.api.move_rand(actor_id)
```

Once the simuation is complete, one more line is logged.
```python
        Logger.info("Agent", "Finished.")
```

For a more in-depth example, the repository also includes an implementation of a (rule-based agent)[../blob/main/agents/rule_based_agent.py]. This agent does complete tasks, queries most information, and calls all relevant commands.