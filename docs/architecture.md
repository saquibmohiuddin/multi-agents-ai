# System Architecture

The **Multi‑Agents AI** framework is organized around three core abstractions:

1. **Agent** – Encapsulates decision‑making logic.  An agent receives observations from the environment and returns actions.  Agents can be stateful (e.g., maintain internal memory) and can communicate with other agents via messages.
2. **Environment** – Represents the external world or simulation.  It defines the observation space, the step function, and the termination condition.  The environment is responsible for feeding observations to agents and applying their joint actions.
3. **Scheduler** – Orchestrates the interaction loop between agents and the environment.  It handles the ordering of agent actions (sequential, parallel, or custom policies) and aggregates the actions into a single dictionary that the environment can consume.

## Interaction Flow

```
+-------------------+      +-------------------+      +-------------------+
|   Environment    |<---->|    Scheduler      |<---->|      Agent(s)    |
+-------------------+      +-------------------+      +-------------------+
        ^   |                       ^   |
        |   v                       |   v
   reset() / step()          schedule_step()  act()
```

1. **Reset** – The environment is reset, producing an initial observation.
2. **Schedule Step** – The scheduler calls each agent’s `act(observation)` method, optionally providing each agent with its own slice of the observation.
3. **Collect Actions** – The scheduler aggregates the actions into a dictionary keyed by agent name.
4. **Environment Step** – The environment receives the joint actions, updates its internal state, and returns the next observation, reward, done flag, and optional info.
5. Loop repeats until `done` is `True`.

## Extensibility Points

- **Custom Agents** – Subclass `Agent` and implement `act`.  You can also override `reset` or add helper methods.
- **Custom Environments** – Subclass `Environment` and implement `reset` and `step`.  This allows integration with existing simulators (e.g., OpenAI Gym, Unity ML‑Agents).
- **Scheduling Policies** – Provide your own `Scheduler` subclass to change execution order, support asynchronous execution, or implement hierarchical coordination.
- **Message Passing** – Agents can exchange messages through a shared `MessageBus` (optional component) for cooperative strategies.

## High‑Level Diagram

```
+-------------------+       +-------------------+       +-------------------+
|   User Code       | ----> |   Multi‑Agents AI | ----> |   External Sim   |
+-------------------+       +-------------------+       +-------------------+
        |                          |                         |
        |   import classes,       |   instantiate          |   run simulation
        |   define agents/envs    |   schedule loop        |
        v                          v                         v
   (your script)            (framework core)          (environment impl)
```

The framework aims to keep the core lightweight while offering hooks for advanced features such as reinforcement‑learning back‑ends, distributed execution, and visualisation tools.
