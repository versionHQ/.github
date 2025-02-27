# Overview

[![DL](https://img.shields.io/badge/Download-20K+-red)](https://clickpy.clickhouse.com/dashboard/versionhq)
![MIT license](https://img.shields.io/badge/License-MIT-green)
[![Publisher](https://github.com/versionHQ/multi-agent-system/actions/workflows/publish.yml/badge.svg)](https://github.com/versionHQ/multi-agent-system/actions/workflows/publish.yml)
![PyPI](https://img.shields.io/badge/PyPI-v1.2.2+-blue)
![python ver](https://img.shields.io/badge/Python-3.11/3.12-purple)
![pyenv ver](https://img.shields.io/badge/pyenv-2.5.0-orange)


Agentic orchestration framework for multi-agent networks and task graphs for complex task automation.

**Visit:**

- [Playground](https://versi0n.io/)
- [Docs](https://docs.versi0n.io)
- [Github](https://github.com/versionHQ/)
- [Python SDK](https://pypi.org/project/versionhq/)

<hr />

## Key Features

`versionhq` is a Python framework for agent networks that handle complex task automation without human interaction.

Model agnostic agents will handle tasks while collaborating with the members in the network by sharing memories, tools, and knowledge sources.


###  Multi-Agent Network

Agents adapt their formation based on task complexity. 

You can specify a desired formation or allow the agents to determine it autonomously (default).


|  | **Solo Agent** | **Supervising** | **Squad** | **Random** |
| :--- | :--- | :--- | :--- | :--- |
| **Formation** | <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1738818211/pj_m_agents/rbgxttfoeqqis1ettlfz.png" alt="solo" width="200"> | <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1738818211/pj_m_agents/zhungor3elxzer5dum10.png" alt="solo" width="200"> | <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1738818211/pj_m_agents/dnusl7iy7kiwkxwlpmg8.png" alt="solo" width="200"> | <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1738818211/pj_m_agents/sndpczatfzbrosxz9ama.png" alt="solo" width="200"> |
| **Usage** | <ul><li>A single agent with tools, knowledge, and memory.</li><li>When self-learning mode is on - it will turn into **Random** formation.</li></ul> | <ul><li>Leader agent gives directions, while sharing its knowledge and memory.</li><li>Subordinates can be solo agents or networks.</li></ul> | <ul><li>Share tasks, knowledge, and memory among network members.</li></ul> | <ul><li>A single agent handles tasks, asking help from other agents without sharing its memory or knowledge.</li></ul> |
| **Use case** | An email agent drafts promo message for the given audience. | The leader agent strategizes an outbound campaign plan and assigns components such as media mix or message creation to subordinate agents. | An email agent and social media agent share the product knowledge and deploy multi-channel outbound campaign. | 1. An email agent drafts promo message for the given audience, asking insights on tones from other email agents which oversee other clusters. 2. An agent calls the external agent to deploy the campaign. |

<hr />

### Graph Theory Concept

To completely automate task workflows, agents will build a `task-oriented network` by generating `nodes` that represent tasks and connecting them with dependency-defining `edges`.

Each node is triggered by specific events and executed by an assigned agent once all dependencies are met.

While the network automatically reconfigures itself, you retain the ability to direct the agents using `should_reform` variable.


The following code snippet demonstrates the `TaskGraph` and its visualization, saving the diagram to the `uploads` directory.

```python
import versionhq as vhq

task_graph = vhq.TaskGraph(directed=False, should_reform=True) # triggering auto formation

task_a = vhq.Task(description="Research Topic")
task_b = vhq.Task(description="Outline Post")
task_c = vhq.Task(description="Write First Draft")

node_a = task_graph.add_task(task=task_a)
node_b = task_graph.add_task(task=task_b)
node_c = task_graph.add_task(task=task_c)

task_graph.add_dependency(
   node_a.identifier, node_b.identifier,
   type=vhq.DependencyType.FINISH_TO_START, weight=5, description="B depends on A"
)
task_graph.add_dependency(
   node_a.identifier, node_c.identifier,
   type=vhq.DependencyType.FINISH_TO_FINISH, lag=1, required=False, weight=3
)

task_graph.visualize()
```

<hr />

### Task Graph

A `TaskGraph` represents tasks as `nodes` and their execution dependencies as `edges`, automating rule-based execution.

`Agent Networks` can handle `TaskGraph` objects by optimizing their formations.

The following example demonstrates a simple concept of a `supervising` agent network handling a task graph with three tasks and one critical edge.

<img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1739337639/pj_m_home/zfg4ccw1m1ww1tpnb0pa.png">

<hr />

## Quick Start

### Package installation

```
pip install versionhq
```

(Python 3.11 / 3.12)


### Launching an agent


```python
import versionhq as vhq

agent = vhq.Agent(role="Marketer")
res = agent.start()

assert isinstance(res, vhq.TaskOutput) # contains agent's response in text, JSON, Pydantic formats with usage recordes and eval scores.
```


### Automating workflows

```python
import versionhq as vhq

network = vhq.form_agent_network(
   task="draft a promo plan",
   expected_outcome="marketing plan, budget, KPI targets",
)
res, tg = network.launch()

assert isinstance(res, vhq.TaskOutput) # the latest output from the workflow
assert isinstance(tg, vhq.TaskGraph) # contains task nodes and edges that connect the nodes with dep-met conditions
```


### Executing a single task

You can simply build and execute a task using `Task` class.

```python
import versionhq as vhq
from pydantic import BaseModel

class CustomOutput(BaseModel):
   test1: str
   test2: list[str]

def dummy_func(message: str, test1: str, test2: list[str]) -> str:
   return f"""{message}: {test1}, {", ".join(test2)}"""

task = vhq.Task(
   description="Amazing task",
   pydantic_output=CustomOutput,
   callback=dummy_func,
   callback_kwargs=dict(message="Hi! Here is the result: ")
)

res = task.execute(context="testing a task function")
assert isinstance(res, vhq.TaskOutput)
```


### Supervising agents

To create an agent network with one or more manager agents, designate members using the `is_manager` tag.

```python
import versionhq as vhq

agent_a = vhq.Agent(role="Member", llm="gpt-4o")
agent_b = vhq.Agent(role="Leader", llm="gemini-2.0")

task_1 = vhq.Task(
   description="Analyze the client's business model.",
   response_fields=[vhq.ResponseField(title="test1", data_type=str, required=True),],
   allow_delegation=True
)

task_2 = vhq.Task(
   description="Define a cohort.",
   response_fields=[vhq.ResponseField(title="test1", data_type=int, required=True),],
   allow_delegation=False
)

network =vhq.AgentNetwork(
   members=[
      vhq.Member(agent=agent_a, is_manager=False, tasks=[task_1]),
      vhq.Member(agent=agent_b, is_manager=True, tasks=[task_2]), # Agent B as a manager
   ],
)
res, tg = network.launch()

assert isinstance(res, vhq.NetworkOutput)
assert not [item for item in task_1.processed_agents if "vhq-Delegated-Agent" == item]
assert [item for item in task_1.processed_agents if "agent b" == item]
```

This will return a list with dictionaries with keys defined in the `ResponseField` of each task.

Tasks can be delegated to a manager, peers within the agent network, or a completely new agent.

<hr />

## Repository Structure

| | LLM Orchestration Framework | Core | Analyics | Use Case |
| :---: | :---: | :---: | :---: | :---: | 
| Github | [repo_a](https://github.com/versionHQ/multi-agent-system) | [repo_b](https://github.com/krik8235/core) | [repo_c](https://github.com/versionHQ/clutering-analysis) | [repo_d](https://github.com/krik8235/pj_m_dev) |
| Website | [PyPI](https://pypi.org/project/versionhq/) | - | - | [client app](https://versi0n.io) |

