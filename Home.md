Welcome to the craft-bots wiki!

## Quick Start

### How to clone and run with a default agent:

CD to the directory of your choice and clone the CraftBots repo:

    git clone https://github.com/strathclyde-artificial-intelligence/craft-bots

CD into the repository and run `main.py`

    cd ./craft-bots
    python main.py

_*Note: CraftBots requires NumPy. Install by running:_

    pip install numpy

This should run CraftBots using `TestAgent`, an agent that moves all actors randomly. 


### How to run with your own agent

Once you have created your own agent (see [here](Creating-an-Agent) on how to make an agent) open `main.py` in the Python IDE of your choice or include the following lines in a separate file.

Start by importing your agent _(assuming your agent is in the agents folder)_ and craftbots:

    from agents.YourAgent import YourAgent
    from craftbots import craft_bots

Call `start_simulation` from `craftbots` with your agent:

    craft_bots.start_simulation(agent_class=YourAgent)

*_Note: You should pass in the constructor for your Agent, not an initialised object!_

To get the score from the simulation, get the return of `start_simulation`
    
    print(craft_bots.start_simulation(agent_class=YourAgent)

## Wiki

- [Craft Bots Rules](Craft-Bots-Rules)

### Creating an agent

  - [Creating an Agent](Creating-an-Agent)
  - [AgentAPI](AgentAPI)


### Initialisation Files

- [Initalisation Files](Initalisation_Files)
  - [Modifiers](Modifiers)
  - [Rules](Rules)
  - [World Gen Modifiers](World-Gen-Modifiers)


### Detailed Documentation

- [World Info](World-Info)
- [Entities](Entities)
  - [Actors](Actors)
  - [Nodes](Nodes)
  - [Edges](Edges)
  - [Resources](Resources)
  - [Mines](Mines)
  - [Sites](Sites)
  - [Buildings](Buildings)
  - [Tasks](Tasks)
  - [Commands](Commands)
