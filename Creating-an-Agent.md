# Intro
This page is meant to guide you through the process of creating an Agent for the CraftBots simulation. Right now, any agent is to be created in Python (3.8). This guide assumes you have already cloned the repo of the main branch and opened it into the IDE of your choice. To see the rules of the simulation from the agents POV, please see the [CraftBots Rules page](https://github.com/strathclyde-artificial-intelligence/craft-bots/wiki/Craft-Bots-Rules)

***

# Structure of the Craftbots Simulation
## File Strucutre
The Craftbots Simulation is split into several different folders regarding different layers of the simulation. The file structure is as follows:
* agents
  * This folder has a few basic agents for the simulation, and it is recommended to keep your agent file here.
* api
  * This folder contains the files for the API through which your agent will interact with the simulation
* craftbots
  * This folder contains the files to manage the simulation, querying your agent, and processing your agents commands and querys to the world, as well as the initialisation files for the simulation, to manage how the world is generated, and the rules of the simulation.
* entities
  * This folder contains the files for the different entities of the simulation. Your agent should not be directly accessing these files, instead interacting with them via the API (more info below) 
* main.py

## Simulation Structure
![Graph showing the architecture of CraftBots](https://i.imgur.com/qYhgcF4.jpg)

In this graph, you can see that the AI Agent only interacts with the Agent API in the simulation, and the main CraftBots file recieves the agent as an object. From there, any queres and commands the agent makes are handled by the CraftBots simulation. 

### Initialisation Files
You also have access to a set of text files in craftbots/initialisation_files. These manage the rules and values of the world generation and simulation. These can be adjusted to create a simulation that works better for your purposes. You can safely edit any file that does not have the prefix "default_". These hold the default and required values for the simulation to run. Any text files whose file names match the default file names without the prefix (e.g. default_modifiers.txt and modifiers.txt) will then overwrite any default values whose keys match. 

For example: In the "default_world_gen_modifiers.txt" file there is a value called MAX_NODES:
`MAX_NODES = 50`

Assuming no changes from the default, the world will look something like this (not exactly, as it is random):

![Big Craftbots Map](https://i.imgur.com/1cWMu2T.png)

If in the "world_gen_modifiers.txt" you have the MAX_NODES value is 5:
`MAX_NODES = 5`

Assuming no other changes from the defaults aside from the one above, the world becomes more like this: 

![Small Craftbots Map](https://i.imgur.com/4pGL6rh.png)

## Agents, AgentAPI, and the Command cycle

![Flow Chart of the Command Cycle](https://i.imgur.com/XDwkNyS.jpg)

The Command Cycle is the system used to allow the agent to determine its actions, even as the world in the simulation progresses. This allows for spending "down-time" thinking and also for the agent to wait to determine the outcomes of its actions before making more decisions. 

At the start of the cycle, the simulation will check if the agent is actively thinking, something that your agent determines. If your agent is not thinking, then the simualtion will create a seperate thread for your agent to think on, by calling a function in your agent called get_next_commands(). 
then the world is updated by a single tick. At this point, actors will make progress towards ongoing actions such as digging, constructing, and moving. Next, if the agent has sent any commands, they are executed in the simulation. Whether successful or not, all actions return a result, usually a true if they are successful and a false otherwise. If there are any results, then the simulation will return them to the agent via the agents (recieve_results(results) command, and it is up to the agent to decide what to do with this information.

When your agent has finished deciding what it will do (assuming it will do so during the simulation), it should change the thinking flag to False, to indicate to the simulation that the agent needs to be called on again. Otherwise, your agent will simply stop thinking and stop performing actions. 

## Structure of an Agent
An agent for the CraftBots simulation is a python object, and has a few required fields / methods to properly interface with the simulation. 

### Fields
The agent needs to have 3 fields:

* `api` will be assigned a reference to an AgentAPI object at the start of the simulation. This object has a set of methods through which your agent can interact with the simulation. Information on how this is done is described below. 
* `thinking` is a flag used by the simulation to determine if your agent is thinking about the commands it will send, and at the start of every tick, this flag is checked. If it is false, it is set to true, and your agent is put on another thread to begin thinking about its next commands via the `get_next_commands()` function 
* `world_info` will be assigned to a dictionary that has all of the relevant information about the world. It is updated each time before the `get_next_commands()` function is called. The agent can also get an up to date version of the dictionary through the API.

### Methods
The agent needs to have 2 methods:

* `recieve_results(results)` this function is called whenever the simulation has new results to give to the agent. The results come in list of tuples with the ID of a command, and its outcome. 
* `get_next_commands()` this function is called whenever the simulation notices that the agent is not thinking. At the end of this function, the agent should set its `thinking` field to False. 

# Creating an Agent
Using the "Structure of an Agent" section above, the bare minimum requirements of an agent are easily found. It appears like so:

    class BlankAgent:
        def __init__(self):
            self.api = None
            self.thinking = False
            self.world_info = None

        def receive_results(self, _):
            pass

        def get_next_commands(self):
            self.thinking = False

Saving this as `blank_agent.py` in the agents folder, we can call in main: 

    from craftbots import craft_bots
    from agents import blank_agent

    if __name__ == '__main__':
        craft_bots.start_simulation(agent=blank_agent.BlankAgent())

And then running this, we start a simulation. 

![CraftBots Simulation Graph with nothing happening](https://i.imgur.com/1cWMu2T.png)

You will notice that nothing is currently moving in the simulation. This is because the only thing that our `get_next_commands()` function does set the agent to stop thinking. This is the bare minimum of an agent, even if it doesn't actually perform anything. This is in the codebase under the file `blank_agent.py`

## Getting Information from the World
Before the agent can begin to make actions in the world, it needs to know what is there. This is what `world_info` and the three API commands `get_world_info()`, `get_by_id()`, and `get_field()` are for. 

`world_info` is updated right before the agent is called on, but represents the world at that point. This is fine when your agent can quickly make decisions and stop thinking, but if your agent waits for certain events / results or just takes a long time to determine its actions, then a new up to date dictionary can be recieved with the `get_world_info()` command from the api. We can change our get_next_commands() function to show how this function is called and how to interact with the API. 

        def get_next_commands(self):
            self.world_info = self.api.get_world_info()
            self.thinking = False

This will update world_info, but nothing else. Still not very helpful.

### Using Autocomplete
If your IDE supports autocomplete for python, then it might be helpful to cast self.api to the AgentAPI class, so methods can easily be accessed by your IDE's auto complete, although this is not necessary for a function agent. To do so: you will need to import agent_api.py into your agent's file like so: `from api import agent_api`, and then at the top of your get_next_commands() function, add the line `self.api: agent_api.AgentAPI`. From there, when writing in the get_next_commands() function, your IDE should provide autocomplete suggestions. 

![Example of the Autocomplete working in PyCharm](https://i.imgur.com/p2axAyg.gif)

This will be used from here down, but is not strictly required

Your agent will most likely want to keep track of the actors. To do this, you can get all of the actors from the world_info dictionary. It is done like so:

        def get_next_commands(self):
            self.api: agent_api.AgentAPI
            print(self.world_info["actors"])
            self.thinking = False

You should be getting output that looks like this:

`{56: {'node': 1, 'state': 0, 'progress': -1, 'id': 56, 'target': None, 'resources': []}, 54: {'node': 23, 'state': 0, 'progress': -1, 'id': 54, 'target': None, 'resources': []}, 55: {'node': 39, 'state': 0, 'progress': -1, 'id': 55, 'target': None, 'resources': []}}`

This is a dictionary with showing the information of 3 actors. You can call them by their ID's. In this case the ID's are 54, 55, and 56. The ID's will be different for each simulation. Perhaps it would be better to see the node that each actor is at. Using `self.api.get_field()` we can do so easily. 

        def get_next_commands(self):
            self.api: agent_api.AgentAPI
            for actor_id in self.world_info["actors"]:
                print(self.api.get_field(actor_id, "node"))
            self.thinking = False

Which will output the ID's of the node each actor is at:
`
33
63
86
`

If we want to see each actor on their own, then we can use `self.api.get_by_id`:
        def get_next_commands(self):
            self.api: agent_api.AgentAPI
            for actor_id in self.world_info["actors"]:
                print(self.api.get_by_id(actor_id))
            self.thinking = False

Output:
`
{'node': 57, 'state': 0, 'progress': -1, 'id': 120, 'target': None, 'resources': []}
{'node': 72, 'state': 0, 'progress': -1, 'id': 122, 'target': None, 'resources': []}
{'node': 85, 'state': 0, 'progress': -1, 'id': 121, 'target': None, 'resources': []}
`

Each entity (that being: Actors, Nodes, Edges, Resources, Mine, Sites, Buildings, Tasks, and Commands) each have a unique, static ID. Knowing an ID is equivalent to have a reference to the object. To have a more indepth explaination of the dictionary layout, see below.

## Sending Commands to Actors

Still, the actors are not moving or doing anything just yet. A good first step would be to start taking your first steps. While the API has several different commands you can give to actors, we will look at one for now: `self.api.move_rand()`. By looping through each actor and sending that command with the actor's ID, we can have each actor move randomly. 

Like so:
        def get_next_commands(self):
            self.api: agent_api.AgentAPI
            for actor_id in self.world_info["actors"]:
                self.api.move_rand(actor_id)
            self.thinking = False

Running the simulation now, you should be able to see the small grey dots moving along the blue lines between the larger nodes:

![CraftBots Simulation with some randomly moving actors](https://i.imgur.com/tnAJxLT.gif)

I would suggest have a look at the `api/agent_api.py` to see what the api offers to your agent and how to use them. If you used the line provided underneath the Autocomplete section, then you can also have your IDE show you all of the methods avalible to you from self.api. 

## Using results
Actions can fail, if conditions are not met such as, your agent sending ID's that don't properly fill the required roles or sending a command under a situation that it cannot be performed. The Command Cycle also informs you aboput the outcomes of your commands. (Using the randomly moving actors from above) We can use the `recieve_results()` function to see the outcomes of our results.

At the end of a tick, whatever commands have been performed will return a tuple with the command ID and the outcome of the action. Like the other entities, Commands also have unique, static ID's. To get the ID, you simply save the return of the command call (this does not apply to `get_world_info`, `get_by_id`, and `get_field`). 

You can see the Command IDs like so:
    print(self.api.move_rand(actor_id))

Output:
`...123
124
125
126
127
128...`

Command IDs are useful if your agent needs to know the outcome of certain events. However, the agent can also just print all of the results it gets back, by having this function:

    def receive_results(self, results):
        print(results)

Running this with our randomly moving agents from above reveals the following:
`
[(123, True), (124, True), (125, True)]
[(126, False), (127, False), (128, False)]
[(129, False), (130, False), (131, False)]
[(132, False), (133, False), (134, False)]
[(135, False), (136, False), (137, False)]
[(138, False), (139, False), (140, False)]
[(141, False), (142, False), (143, False)]
`

There is lots of False (i.e failed) command results? Why? Because the actor cannot move to a random node while it is currently moving. This can be seen through its state. When the state is 0, it is Idle, when it is 1, it is moving. There are other states as well for other ongoing actions. We can use this information to make a decision before sending a command. Changing the `get_next_commands()` to the below function should remedy this:

    def get_next_commands(self):
        self.api: agent_api.AgentAPI
        for actor_id in self.world_info["actors"]:
            if self.api.get_field(actor_id, "state") == 0:
                self.api.move_rand(actor_id)
        self.thinking = False

By checking if the actor is Idle before sending a command, we should greatly reduce the amount of commands sent to the simulation. This out output:

`[(85, True), (86, True), (87, True)]
[(88, True)]
[(89, True)]
[(90, True)]
[(91, True)]
[(92, True)]
[(93, True)]
`

Now all of the commands are successful. 

By checking the API, and the [rules](https://github.com/strathclyde-artificial-intelligence/craft-bots/wiki/Craft-Bots-Rules), you should be able to begin creating an agent for the CraftBots simulation. But what do you do?

## Tasks
### NOTE: EVERYTHING BELOW THIS, ASIDE FROM HOW TO ACCESS THE TASKS, HAS NOT YET BEEN IMPLEMENTED
The goal of Craftbots Simulation is to complete tasks that consist of building a number of structures at certain nodes. These tasks come in online and have a deadline by when they need to be completed. Upon completion, they have a certain point value that your agent then recieves. After 15min, the simulation will stop and the final score for the agent is returned.

To get information on the tasks your agent currently has you can use: `self.world_info["tasks"]`, which will return a dictionary with all of the currently incomplete tasks. Each task is also given a unique, static ID, which can be used to reference it.


