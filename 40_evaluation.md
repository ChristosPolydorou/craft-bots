Agents can be evaluated using the script [evaluation.py](../blob/main/evaluate.py).

```bash
$ python evaluate.py -h
usage: evaluate.py [-h] [-n N] [-f F] [-o O] [-r R]

optional arguments:
  -h, --help  show this help message and exit
  -n N        set number of simulation runs (default 1)
  -f F        configuration file
  -o O        output file
  -r R        override config simulation rate (default -1, no override)
```

## Connecting an agent to the evaluate script.

The agent is created on line 9 of  [evaluation.py](../blob/main/evaluate.py#L9).
```python
def reset_simulation(sim : Simulation):
    sim.agents.clear()
    agent = RBAgent()
    sim.agents.append(agent)
    sim.reset_simulation()
```
Simply replace the RBAgent class with your own adapter.

## Evaluation Information

- **Rate override* Changing the simulation rate can change the performance of your agent, if they are running in seperate threads. Running in lockstep means that the simulation will pause while the agent is thinking, but note that this is incomparable with a real-time execution.
- **Output file** will be a CSV file with headers: "seed", "max score", and "score". Max score is the total score of all tasks which were generated during the simulation, and might not be possible to achieve.