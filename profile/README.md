# version HQ

[![DL](https://img.shields.io/badge/Download-15K+-red)](https://clickpy.clickhouse.com/dashboard/versionhq)
![MIT license](https://img.shields.io/badge/License-MIT-green)
[![Publisher](https://github.com/versionHQ/multi-agent-system/actions/workflows/publish.yml/badge.svg)](https://github.com/versionHQ/multi-agent-system/actions/workflows/publish.yml)
![PyPI](https://img.shields.io/badge/PyPI-v1.1.12+-blue)
![python ver](https://img.shields.io/badge/Python-3.11+-purple)
![pyenv ver](https://img.shields.io/badge/pyenv-2.5.0-orange)
![node](https://img.shields.io/badge/node-22.0-darkblue)


Agentic orchestration framework to deploy agent network and handle complex task automation.

**Visit:**

- [Github](https://github.com/versionHQ/multi-agent-system)
- [Docs](https://docs.versi0n.io)
- [PyPI](https://pypi.org/project/versionhq/)
- [Playground](https://versi0n.io/)


<hr />

## Key Features

`versionhq` is a Python framework that can generate agent networks for complex task automation without human interaction.

Agents are model-agnostic, and will improve task output, while oprimizing token cost and job latency, by sharing their memory, knowledge base, and RAG tools with other agents in the network.


###  Agent formation

Agents adapt their formation based on task complexity.

You can specify a desired formation or allow the agents to determine it autonomously (default).

|  | **Solo Agent** | **Supervising** | **Network** | **Random** |
| :--- | :--- | :--- | :--- | :--- |
| **Formation** | <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1738818211/pj_m_agents/rbgxttfoeqqis1ettlfz.png" alt="solo" width="200"> | <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1738818211/pj_m_agents/zhungor3elxzer5dum10.png" alt="solo" width="200"> | <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1738818211/pj_m_agents/dnusl7iy7kiwkxwlpmg8.png" alt="solo" width="200"> | <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1738818211/pj_m_agents/sndpczatfzbrosxz9ama.png" alt="solo" width="200"> |
| **Usage** | <ul><li>A single agent with tools, knowledge, and memory.</li><li>When self-learning mode is on - it will turn into **Random** formation.</li></ul> | <ul><li>Leader agent gives directions, while sharing its knowledge and memory.</li><li>Subordinates can be solo agents or networks.</li></ul> | <ul><li>Share tasks, knowledge, and memory among network members.</li></ul> | <ul><li>A single agent handles tasks, asking help from other agents without sharing its memory or knowledge.</li></ul> |
| **Use case** | An email agent drafts promo message for the given audience. | The leader agent strategizes an outbound campaign plan and assigns components such as media mix or message creation to subordinate agents. | An email agent and social media agent share the product knowledge and deploy multi-channel outbound campaign. | 1. An email agent drafts promo message for the given audience, asking insights on tones from other email agents which oversee other clusters. 2. An agent calls the external agent to deploy the campaign. |


<hr />

## Quick Start

**Install `versionhq` package:**

   ```
   pip install versionhq
   ```

(Python 3.11 or higher)

### Generate agent networks and execute tasks:

   ```python
   from versionhq import form_agent_network

   network = form_agent_network(
      task="YOUR AMAZING TASK OVERVIEW",
      expected_outcome="YOUR OUTCOME EXPECTATION",
   )
   res = network.launch()
   ```

   This will form a network with multiple agents on `Formation` and return `TaskOutput` object with output in JSON, plane text, and Pydantic model format with evaluation.


### Solo Agent:

If you don't need a network, you can simply build an agent using `Agent` model.

By default, the agent prioritizes JSON serializable outputs over plane text.

   ```python
   from pydantic import BaseModel
   from versionhq import Agent, Task

   class CustomOutput(BaseModel):
      test1: str
      test2: list[str]

   def dummy_func(message: str, test1: str, test2: list[str]) -> str:
      return f"{message}: {test1}, {", ".join(test2)}"


   agent = Agent(role="demo", goal="amazing project goal")

   task = Task(
      description="Amazing task",
      pydantic_output=CustomOutput,
      callback=dummy_func,
      callback_kwargs=dict(message="Hi! Here is the result: ")
   )

   res = task.execute_sync(agent=agent, context="amazing context to consider.")
   print(res)
   ```


This will return a `TaskOutput` object that stores response in plane text, JSON, and Pydantic model: `CustomOutput` formats with a callback result, tool output (if given), and evaluation results (if given).

   ```python
   res == TaskOutput(
      task_id=UUID('<TASK UUID>'),
      raw='{\"test1\":\"random str\", \"test2\":[\"str item 1\", \"str item 2\", \"str item 3\"]}',
      json_dict={'test1': 'random str', 'test2': ['str item 1', 'str item 2', 'str item 3']},
      pydantic=<class '__main__.CustomOutput'>,
      tool_output=None,
      callback_output='Hi! Here is the result: random str, str item 1, str item 2, str item 3', # returned a plain text summary
      evaluation=None
   )
   ```

### Supervising:

   ```python
   from versionhq import Agent, Task, ResponseField, Team, TeamMember

   agent_a = Agent(role="agent a", goal="My amazing goals", llm="llm-of-your-choice")
   agent_b = Agent(role="agent b", goal="My amazing goals", llm="llm-of-your-choice")

   task_1 = Task(
      description="Analyze the client's business model.",
      response_fields=[ResponseField(title="test1", data_type=str, required=True),],
      allow_delegation=True
   )

    task_2 = Task(
      description="Define the cohort.",
      response_fields=[ResponseField(title="test1", data_type=int, required=True),],
      allow_delegation=False
   )

   team = Team(
      members=[
         TeamMember(agent=agent_a, is_manager=False, task=task_1),
         TeamMember(agent=agent_b, is_manager=True, task=task_2),
      ],
   )
   res = team.launch()
   ```

This will return a list with dictionaries with keys defined in the `ResponseField` of each task.

Tasks can be delegated to a team manager, peers in the team, or completely new agent.

<hr />

## UI

<p align="center">
   <img
    src="https://res.cloudinary.com/dfeirxlea/image/upload/v1734942341/pj_m_home/yk9pm42xn0ibdrtpmb0d.png"
    alt="version UI dark mode"
   />
   <img
    src="https://res.cloudinary.com/dfeirxlea/image/upload/v1734943272/pj_m_home/dhubpqhj4qjyqwldkgwk.png"
    alt="pypi package"
   />
</p>

<hr />

## Project Structure

| | LLM Orchestration Framework | Core | Analyics | Use Case |
| :---: | :---: | :---: | :---: | :---: | 
| Github | [repo_a](https://github.com/versionHQ/multi-agent-system) | [repo_b](https://github.com/krik8235/core) | [repo_c](https://github.com/versionHQ/clutering-analysis) | [repo_d](https://github.com/krik8235/pj_m_dev) |
| Website | [PyPI](https://pypi.org/project/versionhq/) | - | - | [client app](https://versi0n.io) |

