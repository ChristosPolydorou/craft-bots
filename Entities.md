# Entities
The CraftBots simulation has a collection of different entities that represent the Craftbots world, and allow your agent to communicate and understand it. There are 8 different entities in the Craftbots simulation:
* [Nodes](https://github.com/strathclyde-artificial-intelligence/craft-bots/wiki/Nodes)
* [Edges](https://github.com/strathclyde-artificial-intelligence/craft-bots/wiki/Edges)
* [Actors](https://github.com/strathclyde-artificial-intelligence/craft-bots/wiki/Actors)
* [Resources](https://github.com/strathclyde-artificial-intelligence/craft-bots/wiki/Resources)
* [Mines](https://github.com/strathclyde-artificial-intelligence/craft-bots/wiki/Mines)
* [Sites](https://github.com/strathclyde-artificial-intelligence/craft-bots/wiki/Sites)
* [Buildings](https://github.com/strathclyde-artificial-intelligence/craft-bots/wiki/Buildings)
* [Tasks](https://github.com/strathclyde-artificial-intelligence/craft-bots/wiki/Tasks)
* [Commands](https://github.com/strathclyde-artificial-intelligence/craft-bots/wiki/Commands)

Each entity, among its properties and a unique ID provided by the simulation, also has a set of fields that an agent can read, stored in a dictionary. Commands sent by the agent to the simulation are also treated as entities, and information about the state of a command can be read in a similar fashion to other entities, via the fields.