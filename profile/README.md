# version HQ

![MIT license](https://img.shields.io/badge/License-MIT-green) 
[![Publisher](https://github.com/versionHQ/multi-agent-system/actions/workflows/publish.yml/badge.svg)](https://github.com/versionHQ/multi-agent-system/actions/workflows/publish.yml) 
![PyPI](https://img.shields.io/badge/PyPI->=v1.1.11-blue)
![python ver](https://img.shields.io/badge/Python->=3.11-purple) 
![pyenv ver](https://img.shields.io/badge/pyenv-2.4.23-orange)
![node](https://img.shields.io/badge/node-22.0-darkblue)


LLM orchestration frameworks to deploy multi-agent systems focusing on complex outbound tasks.

**Visit:**

- [PyPI](https://pypi.org/project/versionhq/)
- [Github (LLM orchestration framework)](https://github.com/versionHQ/multi-agent-system)
- [Github (Test client app)](https://github.com/versionHQ/test-client-app)
- [Use case](https://versi0n.io/playground) / [Quick demo](https://res.cloudinary.com/dfeirxlea/video/upload/v1737732977/pj_m_home/pnsyh5mfvmilwgt0eusa.mov)
- [Documentation](https://chief-oxygen-8a2.notion.site/Documentation-17e923685cf98001a5fad5c4b2acd79b?pvs=4) *Some components are under review.

<hr />

## Key Features

Generate mulit-agent systems depending on the complexity of the task, and execute the task with agents of choice.

Model-agnostic agents can handle RAG tools, tools, callbacks, and knowledge sharing among other agents.


###  Agent formation

Agents adapt their formation based on task complexity. 

You can specify a desired formation or allow the agents to determine it autonomously (default).


|  | **Solo Agent** | **Supervising** | **Network** | **Random** |
| :--- | :--- | :--- | :--- | :--- |
| **Formation** | <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1737893140/pj_m_agents/tglrxoiuv7kk7nzvpe1z.jpg" alt="solo" width="200"> | <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1737893141/pj_m_agents/hxngdvnn5b5qdxo0ayl5.jpg" alt="solo" width="200"> | <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1737893142/pj_m_agents/kyc6neg8m6keyizszcpi.jpg" alt="solo" width="200"> | <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1737893139/pj_m_agents/hzpchlcpnpwxwaapu1hr.jpg" alt="solo" width="200"> |
| **Usage** | <ul><li>A single agent with tools, knowledge, and memory.</li><li>When self-learning mode is on - it will turn into **Random** formation.</li></ul> | <ul><li>Leader agent gives directions, while sharing its knowledge and memory.</li><li>Subordinates can be solo agents or networks.</li></ul> | <ul><li>Share tasks, knowledge, and memory among network members.</li></ul> | <ul><li>A single agent handles tasks, asking help from other agents without sharing its memory or knowledge.</li></ul> |
| **Use case** | An email agent drafts promo message for the given audience. | The leader agent strategizes an outbound campaign plan and assigns components such as media mix or message creation to subordinate agents. | An email agent and social media agent share the product knowledge and deploy multi-channel outbound campaign. | 1. An email agent drafts promo message for the given audience, asking insights on tones from other email agents which oversee other clusters. 2. An agent calls the external agent to deploy the campaign. |

<hr />

## Quick Start

**Install `versionhq` package:**

   ```
   pip install versionhq
   ```

(Python 3.11 or higher)


### Case 1. Solo agent:

#### Return a structured output with a summary in string.

   ```
   from pydantic import BaseModel
   from versionhq.agent.model import Agent
   from versionhq.task.model import Task

   class CustomOutput(BaseModel):
      test1: str
      test2: list[str]

   def dummy_func(message: str, test1: str, test2: list[str]) -> str:
      return f"{message}: {test1}, {", ".join(test2)}"


   agent = Agent(role="demo", goal="amazing project goal")

   task = Task(
      description="Amazing task",
      pydantic_custom_output=CustomOutput,
      callback=dummy_func,
      callback_kwargs=dict(message="Hi! Here is the result: ")
   )

   res = task.execute_sync(agent=agent, context="amazing context to consider.")
   print(res)
   ```

This will return `TaskOutput` that stores a response in string, JSON dict, and Pydantic model: `CustomOutput` formats with a callback result.

   ```
   res == TaskOutput(
      task_id=UUID('<TASK UUID>')
      raw='{\"test1\":\"random str\", \"test2\":[\"str item 1\", \"str item 2\", \"str item 3\"]}',
      json_dict={'test1': 'random str', 'test2': ['str item 1', 'str item 2', 'str item 3']},
      pydantic=<class '__main__.CustomOutput'>
      tool_output=None,
      callback_output='Hi! Here is the result: random str, str item 1, str item 2, str item 3',
      evaluation=None
   )
   ```

### Case 2. Supervising:

   ```
   from versionhq.agent.model import Agent
   from versionhq.task.model import Task, ResponseField
   from versionhq.team.model import Team, TeamMember

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
   res = team.kickoff()
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

