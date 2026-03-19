# Multi-Agent Research & Charting Assistant

This project implements a modular multi-agent system using **LangGraph**, **LangChain**, and **Llama 3.1 (via Groq)**. The system orchestrates multiple specialized agents to perform web research, generate data visualizations (charts), analyze images, and synthesize comprehensive answers.

## Features

- **Planner Agent**: Decomposes complex user queries into step-by-step plans.
- **Executor Agent**: Orchestrates the workflow, deciding which agent to call next based on the plan.
- **Web Researcher**: Fetches real-time data using the Tavily Search API.
- **Chart Generator**: Writes and executes Python code to generate Matplotlib charts (saved as temporary images).
- **Chart Summarizer**: Analyzes generated charts to provide insights.
- **Synthesizer**: Combines all findings into a final, human-readable response.

## Prerequisites

- **Python 3.10+**
- **Jupyter Notebook** environment (VS Code or JupyterLab recommended)
- **Groq API Key**: For the LLM logic (Llama 3.1).
- **Tavily API Key**: For web search capabilities.

## Installation

1.  **Clone the repository** (if applicable) or ensure you have the project files:
    - `MultiAgent_Initialization.ipynb` (Main notebook)
    - `helper.py` (Core logic and tool definitions)
    - `prompts.py` (System prompts for agents)

2.  **Install dependencies**:
    Run the following command in your terminal or a notebook cell:

    ```bash
    pip install langgraph langchain langchain-groq langchain-community langchain-experimental python-dotenv matplotlib pandas tavily-python
    ```

## Configuration

1.  **Create a `.env` file** in the root directory of the project.
2.  Add your API keys to the `.env` file:

    ```env
    GROQ_API_KEY=your_groq_api_key_here
    TAVILY_API_KEY=your_tavily_api_key_here
    ```

    > **Note**: Do not share your `.env` file or commit it to version control.

## Usage

1.  **Open `MultiAgent_Initialization.ipynb`** in VS Code or Jupyter.
2.  **Run the cells** in order to initialize the graph and agents.
3.  **Execute a query**:
    Scroll to the bottom of the notebook to find the invocation cells.

    **Example 1: Chart Generation**
    ```python
    query = "Chart the current market capitalization of the top 5 banks in the US?"
    # ... setup state ...
    graph.invoke(state)
    ```

    **Example 2: Text Research**
    ```python
    query = "Identify current regulatory changes for the financial services industry in the US."
    # ... setup state ...
    graph.invoke(state)
    ```

## Project Structure

- **`MultiAgent_Initialization.ipynb`**: The main entry point. Initializes the state graph, defines the nodes (Planner, Executor, Agents), and runs the workflow.
- **`helper.py`**: Contains helper functions, the `PythonREPL` tool for code execution, and node logic refactored for modularity.
- **`prompts.py`**: storage for system prompts used by the Planner, Executor, and Agents to guide their behavior.

## Troubleshooting

- **Rate Limits**: If you encounter 429 errors from Groq, wait a moment or switch to a smaller model (configured in the notebook).
- **Context Limits**: The web researcher truncates results to ~3000 characters to prevent overflowing the LLM's context window.
- **Chart Issues**: If charts don't appear, ensure `matplotlib` is installed. The system is designed to display ephemeral charts (`temp_chart.png`) and clean them up afterward.
