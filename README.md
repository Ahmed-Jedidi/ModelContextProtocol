<img width="1600" height="1190" alt="image" src="https://github.com/user-attachments/assets/83ca1f7a-501e-4985-89fc-cfd8d66bffa4" />

# ModelContextProtocol
---
# Python MCP Server & Client Implementation

A reference implementation of the **Model Context Protocol (MCP)** featuring a FastMCP server, an LLM-powered client (Claude), and a comprehensive testing suite.

This project demonstrates how to connect a Large Language Model (Anthropic Claude) to local tools (file creation) using the MCP standard, with workflows for both integration and unit testing.
<img width="1900" height="875" alt="image" src="https://github.com/user-attachments/assets/38a42296-c5fd-441f-9f86-2298645d4d4d" />

## 📂 Project Structure

| File | Description |
| --- | --- |
| **`mcp_server.py`** | A FastMCP server exposing a `create_file` tool via `stdio` transport. |
| **`host_client_test.py`** | An interactive CLI client that connects the MCP server to Anthropic's Claude API. |
| **`integration_test.py`** | Tests the full MCP protocol handshake and tool execution via `stdio` (Simulates a real client). |
| **`test_server.py`** | **Unit test** that imports the server module directly to verify tool logic without the protocol overhead. |

## 🛠️ Prerequisites

* **Python 3.10+**
* **uv** (Python package manager)
* **Anthropic API Key**

## 🚀 Installation & Setup

This project uses `uv` for dependency management.

### 1. Install `uv` (Windows)

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

## 💻 Usage

### 1. Run the Interactive Client (Main Application)

This connects the `mcp_server` to Claude. You can ask Claude to create files, and it will use the server's tool to do so.

```bash
uv run host_client_test.py mcp_server.py

```

**Example Session:**

```text
Query: Create a hello world python file called hello.py
...
[Calling tool create_file with args {'file_path': 'hello.py', 'content': 'print("Hello World")'}]

```

### 2. Run Direct Unit Tests (`test_server.py`)

Use this to test the **logic** of your tools directly (bypassing the MCP transport layer). This is useful for debugging the actual Python functions inside your server.

```bash
uv run test_server.py

```

*Expected Output: `✅ Test passed! File was created successfully.*`

### 3. Run Integration Tests (`integration_test.py`)

Use this to test the **protocol connection**. This ensures the server is correctly communicating via Standard IO and that the MCP ClientSession can parse the tools.

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

## 📄 License

[MIT](https://choosealicense.com/licenses/mit/)
