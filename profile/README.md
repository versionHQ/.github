# version HQ

A framework for orchestration and multi-agent system that design, deploy, and autopilot messaging workflows based on performance.

Agents are model agnostic.

Messaging workflows are created at individual level, and will be deployed on third-party services via `Composio`.

**Visit:**

- [Client interface (beta)](https://versi0n.io/)
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
```
from versionhq.agent.model import Agent
agent = Agent(llm="your-llm"...)
```

[pypi package](https://pypi.org/project/versionHQ/)


<hr />


## UI
<p align="center">
    <img alt="UI" src="https://res.cloudinary.com/dfeirxlea/image/upload/v1733414200/pj_m_home/tqgg3xfpk5x4i6rh3egv.png" width="60%">
&nbsp;&nbsp;&nbsp;
   <img src="https://res.cloudinary.com/dfeirxlea/image/upload/v1728302420/pj_m_home/xy58a7imyquuvkgukqxt.png" width="25%" alt="messaging workflow">
</p>
