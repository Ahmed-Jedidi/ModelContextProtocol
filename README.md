# ModelContextProtocol
File creation with MCP
---

# Python MCP Server & Client Implementation

A reference implementation of the **Model Context Protocol (MCP)** featuring a FastMCP server, an LLM-powered client (Claude), and automated integration testing.

This project demonstrates how to connect a Large Language Model (Anthropic Claude) to local tools (file creation) using the MCP standard.
<img width="1900" height="875" alt="image" src="https://github.com/user-attachments/assets/38a42296-c5fd-441f-9f86-2298645d4d4d" />

## 📂 Project Structure

| File | Description |
| --- | --- |
| **`mcp_server.py`** | A FastMCP server exposing a `create_file` tool via `stdio` transport. |
| **`host_client_test.py`** | An interactive CLI client that connects the MCP server to Anthropic's Claude API. |
| **`integration_test.py`** | A standalone integration test to verify server functionality without LLM costs. |

## 🛠️ Prerequisites

* **Python 3.10+**
* **uv** (Python package manager)
* **Anthropic API Key**

## 🚀 Installation & Setup

This project uses `uv` for dependency management.

### 1. Install `uv` (Windows)

If you haven't installed `uv` yet:

```powershell
powershell -ExecutionPolicy ByPass -File uv-installer.ps1
$env:Path = "C:\Users\user\.local\bin;$env:Path"

```

### 2. Initialize Project & Dependencies

Run the following commands to set up the environment and install required packages (`anthropic`, `mcp`, `httpx`):

```bash
# Initialize project
uv init
uv venv

# Activate virtual environment (Windows)
.venv\Scripts\activate.bat

# Install dependencies
uv add anthropic "mcp[cli]" httpx

```

### 3. Environment Configuration

Create a `.env` file in the root directory to store your API key:

```env
ANTHROPIC_API_KEY=your_sk_ant_key_here

```

> **Note:** The client is currently configured to use `claude-haiku-4-5`. If you do not have access to this specific model, please update the `model` parameter in `host_client_test.py` to a standard model like `claude-3-5-sonnet-20241022`.

## 💻 Usage

### 1. Running the Interactive Client

This connects the `mcp_server` to Claude. You can ask Claude to create files, and it will use the server's tool to do so.

```bash
uv run host_client_test.py mcp_server.py

```

**Example Session:**

```text
Query: Create a hello world python file called hello.py
...
[Calling tool create_file with args {'file_path': 'hello.py', 'content': 'print("Hello World")'}]
...
I have created the hello.py file for you.

```

### 2. Running Integration Tests

To verify that the MCP server works correctly (without using the LLM API):

```bash
uv run integration_test.py

```

*Expected Output: `✅ Integration test PASSED!*`

## 🔍 Debugging & Inspection

You can use the official MCP Inspector to visualize and test your server tools via a web interface.

**Prerequisite:** Ensure you have `npx` (Node.js) installed.

```bash
uv run mcp dev ./mcp_server.py

```

This will launch the MCP Inspector at `http://localhost:6274`. You can interactively test the `create_file` tool directly from your browser.

## 🧩 How It Works

1. **Server (`mcp_server.py`)**: Uses `FastMCP` to define a simple toolset. It listens on `stdio` for instructions.
2. **Client (`host_client_test.py`)**:
* Initializes an `MCPClient`.
* Spawns the server script as a subprocess.
* Fetches available tools (`list_tools`).
* Sends user queries + tool definitions to the Anthropic API.
* If Claude decides to use a tool, the client executes it via the MCP session and returns the result to Claude.



## 📄 License

[MIT](https://choosealicense.com/licenses/mit/)
