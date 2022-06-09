Welcome to the craft-bots wiki!

### Quickstart

Install the dependencies, clone the repository, and run main.py
```
git clone https://github.com/strathclyde-artificial-intelligence/craft-bots
cd ./craft-bots
pip install -r requirements.txt
python main.py
```
Press "reset" to generate a new simulation, and "start" to begin the simulation. The default agent acts randomly.

### Connect your own agent

Once you have created your own agent open main.py and modify the lines which append the default agent to the simulation.
```python
    # agent
    agent = TestAgent()
    sim.agents.append(agent)
```

## Contents

- [rules overview](Craft-Bots-Rules)
- [configuration file](Configuration)
- [creating an agent](Creating-an-Agent)
  - [world info](World-Info)
  - [commands](Commands)
- [evaluations](Evaluations)