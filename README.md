# Structured Web Data Extraction Agent

A multi-agent research pipeline built with **LangGraph**, **LangChain**, and **Llama 3.1 (via Groq)**. Its primary idea is to reliably extract, process, and synthesize complex nested web structures—such as deep Reddit forum threads—where standard scrapers or typical web-research agents usually fall short.

By employing an orchestrated approach with human-in-the-loop interruption, the pipeline allows a user to verify the extracted web data before passing it to a final synthesizer.

## Setup & Configuration

1. **Environment Requirements:**
    - **Python 3.10+**
    - Jupyter Notebook environment (VS Code or JupyterLab recommended)

2. **Install Dependencies:**
    ```bash
    pip install langgraph langchain langchain-groq langchain-community python-dotenv tavily-python requests
    ```

3. **API Keys:**
    Create a `.env` file in the root of the project with the following:
    ```env
    GROQ_API_KEY=your_groq_api_key_here
    TAVILY_API_KEY=your_tavily_api_key_here
    ```

## Notebook Versions

As this research evolves, different iterative versions of the Data Agent are created. Here is what each version currently does:

### `DataAgent.ipynb` (Base Version)
- **Core Pipeline:** Implements the baseline multi-agent graph containing a Planner, Executor, Web Researcher, and Synthesizer.
- **Data Scraping:** Uses a custom Reddit JSON extractor fallback for standard URL queries. It grabs the post content and up to the top 15 comments.
- **Synthesis:** Takes the scraped context and generates a detailed summary.
- **Interrupt:** Features a LangGraph interruption (`interrupt_before=["synthesizer"]`) allowing the user to review the scraped text before the LLM finalizes the answers.

### `DataAgent_v2.ipynb` (Goal-Plan-Act & Deep Context)
- **Goal-Plan-Act Alignment:** Upgrades the Planner prompt to explicitly enforce `pre_conditions`, `post_conditions`, and a specific `goal` for each step, improving execution efficiency and keeping the agent strictly on track.
- **No Arbitrary Limits**: Removes the "top 15" restrictions. The agent pushes dynamic boundaries (capturing up to 1,000 comments) and uses a character constraint (~60,000 chars) specifically tailored for Llama 3.1 context limits.
- **Strict Synthesizer Guidelines**: Enforces strict instructions during synthesis to format answers into isolated sections (`Post Content Summary`, `Community Reaction`, `Key Insights`) and aggressively prevents the mention of specific commenter names to maintain privacy and readable abstractions.

### `DataAgent_v3.ipynb` (Autonomous Evaluation & Recovery)
- **Inline Context Evaluations (Idea 1):** Introduces an LLM judge step inside the web research node. It evaluates the "Context Relevance" of the pulled web data using a 0.0 to 1.0 scoring mechanism immediately after scraping to identify empty, deleted, or blocked web pages.
- **Autonomous Replanning (Idea 3):** Hooks the context evaluation scores into a `replan_flag`. If the data extraction yields a critically low score (`< 0.4`), the `executor_node` is programmed to dynamically return control to the Planner to restart and try an alternate research trajectory, granting self-healing properties to the multi-agent graph.

*(Future versions mapped to further iterations of the research will be documented here.)*
