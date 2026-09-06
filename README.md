# Multi-Agents AI

## Overview

**Multi‑Agents AI** is a lightweight Python framework that simplifies the creation, coordination, and execution of multiple autonomous agents. It provides a clear separation between agents, their environment, and the scheduling/communication mechanisms, allowing developers to prototype complex multi‑agent systems quickly.

The library is intentionally minimalistic – it ships with a small core API and is fully extensible, so you can plug in custom agents, environments, message‑passing protocols, or reinforcement‑learning back‑ends.

---

## Installation

```bash
# Install the latest released version from PyPI
pip install multi-agents-ai
```

Or install the development version directly from the repository:

```bash
git clone https://github.com/your-org/multi-agents-ai.git
cd multi-agents-ai
pip install -e .
```

---

## Quick Start

```python
from multi_agents_ai import Agent, Environment, Scheduler

# Define a simple echo agent
class EchoAgent(Agent):
    def act(self, observation):
        # Echo back the observation as the action
        return observation

# Create an environment that provides a numeric observation
class CounterEnv(Environment):
    def __init__(self):
        self.step = 0
    def reset(self):
        self.step = 0
        return self.step
    def step(self, actions):
        self.step += 1
        return self.step, {}, self.step >= 5, {}

# Instantiate components
env = CounterEnv()
agent = EchoAgent(name="echo")
scheduler = Scheduler(agents=[agent])

# Run a simple episode
observation = env.reset()
while True:
    actions = scheduler.step(observation)
    observation, reward, done, info = env.step(actions)
    if done:
        break
print("Episode finished.")
```

The example demonstrates how to:
1. Define custom agents by subclassing `Agent`.
2. Implement an environment by subclassing `Environment`.
3. Use the built‑in `Scheduler` to coordinate agent actions.

---

## Documentation

* **Architecture Overview** – see [`docs/architecture.md`](docs/architecture.md) for a high‑level diagram and description of the core components.
* API reference – generated docs are available on the project's GitHub Pages (link to be added).

---

## Contributing

We welcome contributions! Please follow these steps:

1. **Fork the repository** and create a new branch for your feature or bug‑fix.
2. Write clear, well‑tested code. Add unit tests under the `tests/` directory.
3. Update the documentation if you add new public APIs.
4. Run the test suite:
   ```bash
   pytest
   ```
5. Submit a pull request with a concise description of your changes.

Please see the full contribution guidelines in [`CONTRIBUTING.md`](CONTRIBUTING.md) (to be added).

---

## License

This project is licensed under the MIT License – see the `LICENSE` file for details.