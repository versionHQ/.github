# version HQ

![MIT license](https://img.shields.io/badge/License-MIT-green) 
[![Publisher](https://github.com/versionHQ/multi-agent-system/actions/workflows/publish.yml/badge.svg)](https://github.com/versionHQ/multi-agent-system/actions/workflows/publish.yml) 
![PyPI](https://img.shields.io/badge/pypi-v1.1.7.2-blue)
![python ver](https://img.shields.io/badge/Python-3.12/3.13-purple) 
![pyenv ver](https://img.shields.io/badge/pyenv-2.4.23-orange)
![node](https://img.shields.io/badge/node-22.0-darkblue)


A framework for orchestration and multi-agent system that design, deploy, and autopilot messaging workflows based on performance.

Agents are model agnostic.

Messaging workflows are created at individual level, and will be deployed on third-party services via `Composio`.

**Visit:**

- [PyPI Package](https://pypi.org/project/versionhq/)
```
pip install versionhq
```

- [Client app (alpha)](https://versi0n.io/)
- [Landing page](https://home.versi0n.io)



<hr />

## Overall Project Structure

|  | Marketing Page | Client App (Beta) | Test app | LLM Orchestration Framework | Core | Analyics |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Github |  [repo_1](https://github.com/krik8235/pj_m_dev_home) | [repo_a](https://github.com/krik8235/pj_m_dev) | [repo_b](https://github.com/versionHQ/test-client-app) | [repo_c](https://github.com/versionHQ/multi-agent-system)  | [repo_d](https://github.com/krik8235/core)| [repo_e](https://github.com/versionHQ/clutering-analysis) |
| Website | [home](https://home.versi0n.io) | [client app](https://versi0n.io) | (localhost) | - | - | - |


## Mindmap

LLM-powered `agents` and `teams` utilize `tools` and their expertise to fulfill client or system-assigned `tasks`.

<p align="center">
   <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1733556715/pj_m_home/urwte15at3h0dr8mdlyo.png" alt="mindmap" width="400">
</p>


## Installing `versionhq` Module (Alpha)

1. Open another terminal, set your repository as root, and run
```
pip install versionhq
```

2. You can use the `versionhq` module in your Python app.
  - **i.e.,** Make LLM-based agent execute the task and return JSON dict.

   ```
   from versionhq.agent.model import Agent
   from versionhq.task.model import Task, ResponseField

   agent = Agent(
      role="demo",
      goal="amazing project goal",
      skillsets=["skill_1", "skill_2", ],
      llm="llm-of-choice"
   )

   task = Task(
      description="Amazing task",
      expected_output_json=True,
      expected_output_pydantic=False,
      output_field_list=[
         ResponseField(title="test1", type=str, required=True),
         ResponseField(title="test2", type=list, required=True),
      ],
      context=["amazing context",],
      tools=["amazing tool"],
      callback=None,
   )

   res = task.execute_sync(agent=agent)
   return res.to_dict()
   ```

This will return a dictionary with keys defined in the ResponseField.

   ```
   { test1: "answer1", "test2": ["answer2-1", "answer2-2", "answer2-3",] }
   ```

For more info: [PyPI package](https://pypi.org/project/versionhq/)


<hr />


## UI
<p align="center">
    <img alt="UI" src="https://res.cloudinary.com/dfeirxlea/image/upload/v1733414200/pj_m_home/tqgg3xfpk5x4i6rh3egv.png" width="60%">
&nbsp;&nbsp;&nbsp;
   <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1728302420/pj_m_home/xy58a7imyquuvkgukqxt.png" width="25%" alt="messaging workflow">
</p>
