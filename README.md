# Dynatrace AI Agent: n8n Workflow for Grail & Automation

This repository contains a professional **n8n workflow** designed for Dynatrace Solution Engineers and SREs. It leverages an AI Agent to interact with **Dynatrace Grail**, perform advanced DQL queries for performance data and BizEvents, and trigger relevant **Dynatrace Automations** via the Model Context Protocol (MCP).

---

## 🚀 Overview

Modern observability requires moving from "seeing" to "acting." This integration allows users to interact with their Dynatrace environment using natural language through a chat interface. The AI Agent intelligently decides whether to query data from Grail or execute an automation workflow based on the real-time state of the system.

### Key Use Cases:
* **Performance Analysis:** Query metrics and logs directly using DQL via the AI Agent.
* **BizEvents Intelligence:** Analyze business events stored in Grail for real-time business impact.
* **Automated Remediation:** The agent detects patterns and checks for existing Dynatrace Automations to trigger.
* **MCP Integration:** Uses the Model Context Protocol to bridge the gap between LLMs and Dynatrace-specific APIs.

---

## 🏗️ Workflow Architecture

The workflow is built on a modular "Agent-Tool" architecture within n8n:

![n8n Workflow Setup](dynatrace_n8n.png)
*Figure 1: High-level view of the AI Agent node, memory, and Dynatrace tool connectors.*

### Core Components:
| Node | Description |
| :--- | :--- |
| **Chat Message Trigger** | The entry point for user interaction via bot agents. |
| **AI Agent** | The "Brain" (Tool Agent) that manages logic and tool selection. |
| **Google Gemini Model** | The LLM powering the reasoning and DQL generation. |
| **Simple Memory** | Provides conversation context, allowing for follow-up questions. |
| **Dynatrace_MCP** | The specialized connector for querying Grail (Performance & BizEvents). |
| **Automation Tools** | Custom nodes for `get_automation` and `run_automation` via Dynatrace APIs. |

---

## 🛠️ Prerequisites

Before importing the workflow, ensure you have:
1. **n8n instance** (Self-hosted or Cloud).
2. **Dynatrace Environment** with Grail enabled.
3. **API Token** with the following scopes:
    * `storage:logs:read`
    * `storage:metrics:read`
    * `storage:bizevents:read`
    * `automation:workflows:read`
    * `automation:workflows:write`
    * `mcp-gateway:servers:invoke`
    * `mcp-gateway:servers:read`
4. **Google Gemini API Key** for the Chat Model in this example (you can use any chat model available).

---

## 🔧 Installation & Setup

1. **Import the Workflow:**
   Download the `dynatrace_n8n_agent.json` file from this repo and import it into your n8n canvas.

2. **Configure AI Model, MCP and Dynatrace Platform tenant Credentials:**
   * Set up your **Google Gemini Chat** credentials.
   * Set up your **MCP Dynatrace** credentials.
   * Configure the HTTP Request nodes for the Dynatrace API using your **Tenant URL** and **API Token**.
   
---

## 📊 How It Works

### 1. Querying Grail
The agent uses the **Dynatrace_MCP** tool to translate natural language into DQL. 
> **User:** "How many 'failed' checkout events happened in the last 15 minutes? or Are there any active problems detected in Dynatrace in the last 30 minutes?"
> **Agent:** [Executes DQL on BizEvents] "There were 42 failed checkouts detected."

### 2. Identifying Automations
Based on the events detected, the agent queries the `get_automation` tool to find workflows tagged with relevant keywords (e.g., "remediation", "restart").

### 3. Execution
If a match is found, the agent can trigger the `run_automation` tool to resolve the issue directly from the chat.

---

## 📸 Screenshots

### AI Agent Logic
![Agent Detail](img/automation_tools.png)
*Detailed view of how the Agent selects between Grail and Automation tools.*

### Sample Interaction
![Chat Sample](img/mcp_dql_executions.png)
*Example of a user querying BizEvents and the agent suggesting an automation.*

---

## 🤝 Contributing
Feel free to submit PRs for new tool definitions or improved DQL prompting templates.
