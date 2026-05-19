# Planner-Agent

An agentic AI blog writing assistant built with **LangGraph** that autonomously plans, researches, and writes long-form blog content using a multi-node graph of specialized LLM agents — powered locally by **Ollama**.

> ⚠️ Status: Work in Progress. Planner and Researcher nodes implemented; Writer and Image-generation nodes in active development.

## Architecture

        ┌─────────┐
START → │ Planner │ → generates blog outline
        └────┬────┘
             ↓
       ┌────────────┐
       │ Researcher │ → fetches sources via Tavily
       └─────┬──────┘
             ↓
         ┌────────┐
         │ Writer │ → drafts sections grounded on research
         └───┬────┘
             ↓
       ┌────────────┐
       │ Image Gen  │ → generates section visuals
       └─────┬──────┘
             ↓
            END

State flows through a `TypedDict` schema with conditional edges enabling iterative refinement between Researcher and Writer.

## Tech Stack

| Layer | Tools |
|---|---|
| Agent orchestration | LangGraph, LangChain |
| LLM (local inference) | LLaMA 3.2 via Ollama |
| Tools | Tavily Search API |
| Schemas | Pydantic |
| Frontend | Streamlit |
| Backend | FastAPI (async REST) |
| Observability | LangSmith |
| Deployment | Docker, AWS EC2 (planned) |

## Features

- Multi-node LangGraph workflow with stateful agent passing
- Local LLM inference via Ollama — no external API costs, full data privacy
- Tool-using agents with real-time web search grounding (Tavily)
- Pydantic schemas for structured LLM output
- Role-specific system prompts per agent node
- Conditional routing for self-correction loops
- LangSmith tracing for debugging multi-agent runs

## Setup

### 1. Install Ollama and pull the model

    # Install Ollama from https://ollama.com
    ollama pull llama3.2
    ollama serve

### 2. Clone and install dependencies

    git clone https://github.com/himanshuy08/Planner-Agent.git
    cd Planner-Agent
    pip install -r requirements.txt

### 3. Configure environment

Create a `.env` file:

    TAVILY_API_KEY=your_tavily_key
    LANGSMITH_API_KEY=your_langsmith_key
    LANGSMITH_TRACING=true
    OLLAMA_BASE_URL=http://localhost:11434
    OLLAMA_MODEL=llama3.2

### 4. Run

    streamlit run app.py

## Project Structure

    Planner-Agent/
    ├── notebooks/          # Experimental notebooks for each agent
    ├── agents/             # Node implementations (planner, researcher, writer)
    ├── graph.py            # LangGraph workflow definition
    ├── schemas.py          # Pydantic models for agent state
    ├── app.py              # Streamlit frontend
    ├── backend.py          # FastAPI REST API (in progress)
    └── requirements.txt

## Roadmap

- [x] Planner node with outline generation
- [x] Researcher node with Tavily integration
- [ ] Writer node with research-grounded drafting
- [ ] Image-generation node
- [ ] FastAPI backend with async `/generate` endpoint
- [ ] Dockerize and deploy on AWS EC2
- [ ] Add evaluation harness for output quality

## References

- [LangGraph docs](https://langchain-ai.github.io/langgraph/)
- [Ollama](https://ollama.com)
- [Tavily Search API](https://tavily.com/)
- Inspired by CampusX's LangGraph agent tutorials
