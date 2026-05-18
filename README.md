# sau1ron 

An autonomous agent application built utilizing the **OpenClaw** framework, engineered to explore event-driven AI workflows and local agent orchestration directly through Discord.

##  Overview
`sau1ron` marks my first deep dive into the AI agent rabbit hole. Moving past static chat completions, this repository explores **agentic loops** and context routing. By leveraging OpenClaw's architecture, the application captures real-time communication events and passes normalized payloads through an execution gateway powered by state-of-the-art LLMs.

##  Tech Stack & Architecture
- **Framework:** OpenClaw (Node.js agentic runtime)
- **Interface/Channel:** Discord API
- **Execution Environment:** Windows / Node.js environment
- **Core Concepts Explored:** Channel normalization, stateful sessions, tool execution policy, and persistent local memory profiles.

##  Key Learnings
- **Orchestration vs Inference:** Gained hands-on experience setting up a control plane (Gateway) between raw user strings and LLM endpoints to queue and serialize incoming states.
- **Autonomy & Guardrails:** Configured workspace permissions to test how an agent handles automated responses while navigating DM policies and security boundaries.

##  Project Structure
- `/skills` - Custom tool descriptions and prompt overrides (`SKILL.md`).
- Configuration files managing connection gateways, model failovers, and session lifecycles.


##  Operational Commands Used

To build, test, and maintain this agent locally, the following core `openclaw` CLI commands are utilized:

* `openclaw onboard` — Initiates the interactive configuration wizard to name the agent, link API keys, and deploy the persistent background service daemon (`systemd`/LaunchAgent).
* `openclaw CLI channels add` — Navigates the channel onboarding suite to pair the local gateway securely with Discord using bot token authorization.
* `openclaw gateway status` — Monitors the health, runtime status, and websocket connections of the background agent plane.
* `openclaw doctor` — Audits the active channel configuration, ensuring that DM policies (`dmPolicy="open"`) and channel allowlists are configured securely to block unauthorized remote command execution.

---

##  How Sau1ron Analyzes Inbound Questions

When a message hits a monitored Discord channel, `sau1ron` doesn't just pass raw text to an LLM. It parses the prompt using a structured NLP pipeline:

1.  **Intent Parsing:** It checks if the input is a system command override (e.g., `/status`, `/reset`, `/think`) or a natural language query.
2.  **Context Assembly:** It strips raw Discord formatting and packages the message alongside an optimized context payload consisting of:
    * **The System Persona (`SOUL.md`):** Injecting its foundational constraints, engineering identity, and behavior guidelines.
    * **The Global Rules (`AGENTS.md` / `TOOLS.md`):** Defining exactly what actions the agent is permitted to perform locally.

---

##  Where the Agent Gets Its Answers

`sau1ron` relies on a **Local-First, Hybrid Knowledge Retrieval** architecture. Instead of pulling from a closed cloud server, it answers from three distinct layers:

### 1. Local Workspace Transcripts (Short-Term Memory)
The agent maintains its conversation state locally inside your machine's workspace directory (`~/.openclaw/workspace/`). It reads from active, streaming text profiles and JSONL transaction logs to maintain multi-turn context across a chat session without suffering from stateless memory loss.

### 2. Distilled Workspace Profiles (Long-Term Memory)
As conversations wrap up, the framework compresses core facts, user preferences, and structural logs down into permanent local Markdown files (`MEMORY.md`). Before running an inference cycle, the agent reads this file to remember previously learned parameters.

### 3. Dynamic Tool & Skill Execution (Real-Time Data)
If a user asks for information outside its local context, `sau1ron` executes local **Modular Skills** defined in `SKILL.md` directories. It can:
* Read and parse files from its local filesystem workspace.
* Trigger shell execution logs to pull runtime environments.
* Perform targeted web search scraping to ingest real-time code documentation or data.
