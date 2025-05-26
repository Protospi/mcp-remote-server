# MCP Research Project

This project consists of two main components: a research server that handles paper retrieval and a chatbot client that interacts with the research server using the Model Context Protocol (MCP).

## Project Structure

```
mcp_project/
├── research_server.py    # Server component for paper retrieval
├── mcp_chatbot.py       # Client chatbot component
├── .venv/               # Virtual environment directory
└── papers/              # Directory for downloaded papers
```

## Server Component (research_server.py)

### Prerequisites
- Python 3.x
- UV package manager
- Node.js and NPM (for the inspector)

### Setup Instructions

1. Navigate to the project directory:
```bash
cd mcp_project_prompt_resources
```

2. Initialize with UV:
```bash
uv init
```

3. Create and activate virtual environment:
```bash
uv venv
source .venv/bin/activate
```

4. Install required dependencies:
```bash
uv add mcp arxiv
uv pip install -r requirements.txt
```

### Running the Server

1. Launch the inspector:
```bash
npx @modelcontextprotocol/inspector uv run research_server.py
```

2. When prompted to install packages, type `y`

3. Configure the Inspector:
   - Open the provided inspector address in your browser
   - Under configuration, specify the Inspector Proxy Address
   - Follow the Inspector UI Instructions for detailed configuration steps

### Accessing Papers
- Navigate to File > Open > L4 > mcp_project
- Papers will be stored in the papers/ directory

### Stopping the Server
- Use `Ctrl+C` in the terminal to stop the inspector

## Client Component (mcp_chatbot.py)

### Prerequisites
- Python 3.x
- UV package manager
- Active virtual environment from server setup

### Setup Instructions

1. Navigate to the project directory:
```bash
cd mcp_project_prompt_resources
```

2. Activate the virtual environment:
```bash
source .venv/bin/activate
```

3. Install additional dependencies:
```bash
uv add anthropic python-dotenv nest_asyncio
```

### Running the Chatbot

1. Start the chatbot:
```bash
uv run mcp_chatbot.py
```

2. Interact with the chatbot through the command line
3. Type `quit` to exit the chatbot

### Accessing Papers
- Navigate to File > Open > L5 > mcp_project to access downloaded papers

## Environment Variables

The chatbot requires the following environment variables:
- `ANTHROPIC_API_KEY`: Your Anthropic API key for Claude access

## Notes

- Ensure the server is running before starting the chatbot
- Keep track of API usage and paper downloads
- The server and client components should be run in separate terminals
- Make sure to properly close both applications when done

## Resources and Prompt Templates

### Paper Storage Structure
The research server organizes papers in the following structure:
- `papers/` - Root directory for all research papers
  - `{topic_name}/` - Folders for each research topic (e.g. ai_interpretability, llm_reasoning)
    - `papers_info.json` - JSON file containing metadata about papers in that topic

### MCP Resources
Resources in MCP are read-only data endpoints similar to GET endpoints in REST APIs. The research server exposes two main resources:

1. List of available topic folders:
   - URI: `papers://folders`
   - Returns list of available research topics

2. Papers information under a specific topic:
   - URI: `papers://{topic}`
   - Returns paper metadata for the specified topic
   - Dynamic URI where {topic} is specified at runtime

### Prompt Templates
The server provides customizable prompt templates using the `@mcp.prompt()` decorator. These templates can be used to generate structured prompts for specific use cases like paper searches.

### Updated Chatbot Commands

The chatbot supports the following special commands:

#### Resource Commands
- `@folders` - List all available research topics
- `@topic` - Get papers information for a specific topic

#### Prompt Commands
- `/prompts` - List all available prompt templates
- `/prompt <name> <arg1=value1>` - Execute a specific prompt with arguments

### Implementation Details

The chatbot has been updated with new methods to handle these features:

1. `connect_to_server`: 
   - Requests available resources and prompt templates
   - Stores resource URIs and prompt names

2. `chat_loop`:
   - Processes special commands (@folders, @topic, /prompts, /prompt)
   - Routes commands to appropriate handlers

3. New Methods:
   - `get_resource`: Handles resource requests
   - `execute_prompt`: Processes prompt template execution
   - `list_prompts`: Shows available prompt templates

Remember to check the complete implementation in the mcp_project folder for detailed usage examples.

Make sure to interact with the chatbot. Here are some query examples:

* @folders
* @ai_interpretability
* /prompts
* /prompt generate_search_prompt topic=history num_papers=2
