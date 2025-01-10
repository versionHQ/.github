# version HQ

![MIT license](https://img.shields.io/badge/License-MIT-green) 
[![Publisher](https://github.com/versionHQ/multi-agent-system/actions/workflows/publish.yml/badge.svg)](https://github.com/versionHQ/multi-agent-system/actions/workflows/publish.yml) 
![PyPI](https://img.shields.io/badge/PyPI-v1.1.7.9-blue)
![python ver](https://img.shields.io/badge/Python-3.12/3.13-purple) 
![pyenv ver](https://img.shields.io/badge/pyenv-2.4.23-orange)
![node](https://img.shields.io/badge/node-22.0-darkblue)


A framework for orchestration and multi-agent system that design, deploy, and autopilot messaging workflows based on performance.

Agents are model agnostic.

Messaging workflows are created at individual level, and will be deployed on third-party services via `Composio`.

**Visit:**

- [PyPI](https://pypi.org/project/versionhq/)
- [Github (LLM orchestration framework)](https://github.com/versionHQ/multi-agent-system)
- [Github (Test client app)](https://github.com/versionHQ/test-client-app)
- [Use case](https://versi0n.io/playground)


<hr />

## Overall Project Structure

| | LLM Orchestration Framework | Core | Analyics | Use Case | Test app |
| :---: | :---: | :---: | :---: | :---: | :---: |
| Github | [repo_a](https://github.com/versionHQ/multi-agent-system) | [repo_b](https://github.com/krik8235/core) | [repo_c](https://github.com/versionHQ/clutering-analysis) | [repo_d](https://github.com/krik8235/pj_m_dev) | [repo_e](https://github.com/versionHQ/test-client-app) | 
| Website | [PyPI](https://pypi.org/project/versionhq/) | - | - | [client app](https://versi0n.io) | - |



## Mindmap

LLM-powered `agents` and `teams` utilize `tools` and their expertise to fulfill client or system-assigned `tasks`.

<p align="center">
   <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1733556715/pj_m_home/urwte15at3h0dr8mdlyo.png" alt="mindmap" width="400">
</p>


## Usage

1. Install `versionhq` package:
   ```
   uv pip install versionhq
   ```

2. You can use the `versionhq` module in your Python app.


### Case 1. Build an AI agent on LLM of your choice and execute a task:

   ```
   from versionhq.agent.model import Agent
   from versionhq.task.model import Task, ResponseField

   agent = Agent(
      role="demo",
      goal="amazing project goal",
      skillsets=["skill_1", "skill_2", ],
      tools=["amazing RAG tool",]
      llm="llm-of-your-choice"
   )

   task = Task(
      description="Amazing task",
      output_field_list=[
         ResponseField(title="test1", type=str, required=True),
         ResponseField(title="test2", type=list, required=True),
      ],
   )
   res = task.execute_sync(agent=agent, context="amazing context to consider.")
   return res.to_dict()
   ```

This will return a dictionary with keys defined in the `ResponseField`.

   ```
   { test1: "answer1", "test2": ["answer2-1", "answer2-2", "answer2-3",] }
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
    <img alt="UI" src="https://res.cloudinary.com/dfeirxlea/image/upload/v1733414200/pj_m_home/tqgg3xfpk5x4i6rh3egv.png" width="60%">
&nbsp;&nbsp;&nbsp;
   <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1728302420/pj_m_home/xy58a7imyquuvkgukqxt.png" width="25%" alt="messaging workflow">
</p>
