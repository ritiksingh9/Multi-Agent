# Multi-Agent Travel Booking System

An AI travel planning application built with LangGraph, LangChain, Groq, Tavily, AviationStack, PostgreSQL, and Streamlit.

The workflow uses separate agents to:

1. Search for flight information.
2. Find hotel information.
3. Create a complete itinerary.
4. Generate a final travel response.

Conversation checkpoints are stored in PostgreSQL so the application can maintain long-term memory by thread ID.

## Project Structure

```text
.
├── frontend.py           # Streamlit user interface
├── main.py               # LangGraph workflow and CLI entry point
├── tools/
│   ├── flight_tool.py    # AviationStack flight search
│   └── tavily_tool.py    # Tavily hotel/web search
└── travel_plans/         # Generated plans (ignored by Git)
```

## Requirements

- Python 3.11+
- PostgreSQL
- Groq API key
- Tavily API key
- AviationStack API key

## Installation

Create and activate a virtual environment, then install the dependencies:

```bash
python -m venv .venv
```

Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

Install packages:

```bash
pip install langgraph langchain langchain-openai langchain-groq langchain-community langchain-tavily psycopg[binary] psycopg_pool python-dotenv tavily-python requests streamlit
```

## Environment Variables

Copy `.env.example` to `.env` and fill in your own credentials. Never commit `.env` to GitHub.

```env
GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key
AVIATIONSTACK_API_KEY=your_aviationstack_api_key
DATABASE_URL=postgresql://username:password@localhost:5432/langgraph_memory
```

Create the PostgreSQL database before running the application:

```sql
CREATE DATABASE langgraph_memory;
```

## Run the Application

Start the Streamlit interface:

```bash
streamlit run frontend.py
```

Or run the command-line workflow:

```bash
python main.py
```

## Deploy on Render

1. Push this project to GitHub, including `requirements.txt` and `render.yaml`. Do not commit `.env`.
2. In Render, choose **New > Blueprint**, connect the GitHub repository, and deploy the `render.yaml` blueprint.
3. In the created web service, add `GROQ_API_KEY`, `TAVILY_API_KEY`, and `AVIATIONSTACK_API_KEY` under **Environment**. `DATABASE_URL` is connected automatically to the Render PostgreSQL database.
4. Redeploy, then open the generated `onrender.com` URL.

The web service starts with:

```bash
streamlit run frontend.py --server.address 0.0.0.0 --server.port $PORT
```

Enter a request such as:

```text
Plan a 5-day trip from Delhi to Dubai with flights, hotels, and an itinerary.
```

## Security

API keys and database credentials must stay in `.env`. If a credential is ever exposed publicly, revoke it and create a replacement immediately.
