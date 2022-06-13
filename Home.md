Welcome to the craft-bots wiki!

## Wiki Contents

- [rules overview](rules_overview)
- [configuration file](configuration)
- [creating an agent](creating_an_agent)
  - [world info](world_info)
  - [commands](commands)
- [evaluation](evaluation)

## Quickstart

Install the dependencies, clone the repository, and run main.py
```
git clone https://github.com/strathclyde-artificial-intelligence/craft-bots
cd ./craft-bots
pip install -r requirements.txt
python main.py
```
Press "reset" to generate a new simulation, and "start" to begin the simulation. The default agent acts randomly.

## Connect your own agent

Once you have created your own agent open main.py and modify the lines which append the default agent to the simulation.
```python
# agent
agent = TestAgent()
sim.agents.append(agent)
```
