# version HQ

![MIT license](https://img.shields.io/badge/License-MIT-green) 
[![Publisher](https://github.com/versionHQ/multi-agent-system/actions/workflows/publish.yml/badge.svg)](https://github.com/versionHQ/multi-agent-system/actions/workflows/publish.yml) 
![PyPI](https://img.shields.io/badge/PyPI->=v1.1.10-blue)
![python ver](https://img.shields.io/badge/Python-3.12/3.13-purple) 
![pyenv ver](https://img.shields.io/badge/pyenv-2.4.23-orange)
![node](https://img.shields.io/badge/node-22.0-darkblue)


LLM orchestration frameworks to deploy multi-agent systems focusing on complex outbound tasks.

**Visit:**

- [PyPI](https://pypi.org/project/versionhq/)
- [Github (LLM orchestration framework)](https://github.com/versionHQ/multi-agent-system)
- [Github (Test client app)](https://github.com/versionHQ/test-client-app)
- [Use case](https://versi0n.io/playground) / [Quick demo](https://res.cloudinary.com/dfeirxlea/video/upload/v1737728699/pj_m_home/k8mprugoyhlo9cuygyf8.mov)

<hr />

## Mindmap
Model-agnostic agents will collaborate to execute complex tasks, levaraging retrieval augmented generation (RAG) tools, shared knowledge bases, and adaptive learning.

<p align="center">
   <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1733556715/pj_m_home/urwte15at3h0dr8mdlyo.png" alt="mindmap" width="300">
</p>

<hr />

## Quick Start

**Install `versionhq` package:**

   ```
   pip install versionhq
   ```

(Python >= 3.13)


### Case 1. Single agent network:

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
      raw="{\\"test1\\": \\"random str\\", \\"test2\\": [\\"item1\\", \\"item2\\"]}",
      json_dict={"test1": "random str", "test2": ["item1", "item2"]},
      pydantic=CustomOutput(test1="random str", test2=["item 1", "item 2"]),
      callback_output="Hi! Here is the result: random str, item 1, item 2",
   )
   ```

### Case 2. Form a team to handle multiple tasks:

   ```
   from versionhq.agent.model import Agent
   from versionhq.task.model import Task, ResponseField
   from versionhq.team.model import Team, TeamMember

   agent_a = Agent(role="agent a", goal="My amazing goals", llm="llm-of-your-choice")
   agent_b = Agent(role="agent b", goal="My amazing goals", llm="llm-of-your-choice")

   task_1 = Task(
      description="Analyze the client's business model.",
      output_field_list=[ResponseField(title="test1", type=str, required=True),],
      allow_delegation=True
   )

    task_2 = Task(
      description="Define the cohort.",
      output_field_list=[ResponseField(title="test1", type=int, required=True),],
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

| | LLM Orchestration Framework | Core | Analyics | Use Case | Test app |
| :---: | :---: | :---: | :---: | :---: | :---: |
| Github | [repo_a](https://github.com/versionHQ/multi-agent-system) | [repo_b](https://github.com/krik8235/core) | [repo_c](https://github.com/versionHQ/clutering-analysis) | [repo_d](https://github.com/krik8235/pj_m_dev) | [repo_e](https://github.com/versionHQ/test-client-app) | 
| Website | [PyPI](https://pypi.org/project/versionhq/) | - | - | [client app](https://versi0n.io) | - |

