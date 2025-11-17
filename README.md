🚀 Your First AI Agent with Google ADK

A complete walkthrough of building your first AI Agent using Google’s Agent Development Kit (ADK) inside a Kaggle Notebook.
This project mirrors the structure of the Kaggle 5-Day Agents Course and demonstrates agent setup, tool usage, debugging, and Web UI integration.

📌 Overview

This repository shows how to:

Install and configure Google ADK

Authenticate with Gemini API Keys

Build a simple agent that can think, act, and use tools

Execute agents using the InMemoryRunner

Explore and debug agents via the ADK Web Interface

✨ Features

Gemini API key setup

Pre-configured ADK environment

Helper utility functions for Kaggle Notebooks

Retry logic with exponential backoff

Agent with Google Search tool enabled

Hands-on examples with real prompts

ADK Web UI setup for debugging and testing

🛠️ 1. Setup
1.1 Optional Installation (Local Only)

Kaggle comes with ADK pre-installed.
To install manually:

pip install google-adk

1.2 Configure Gemini API Key

Create an API key → Google AI Studio

Add the key to Kaggle Secrets as GOOGLE_API_KEY

Authenticate in the notebook:

from kaggle_secrets import UserSecretsClient
import os

GOOGLE_API_KEY = UserSecretsClient().get_secret("GOOGLE_API_KEY")
os.environ["GOOGLE_API_KEY"] = GOOGLE_API_KEY

📦 1.3 Import Required Components
from google.adk.agents import Agent
from google.adk.models.google_llm import Gemini
from google.adk.runners import InMemoryRunner
from google.adk.tools import google_search
from google.genai import types

🔧 1.4 Helper Functions

This project includes utility functions for:

Proxy URL generation

ADK Web UI access

Kaggle-specific notebook handling

These ensure the ADK Web Interface works seamlessly.

🔁 1.5 Retry Options

Retry configuration to handle rate limits and temporary errors:

retry_config = types.HttpRetryOptions(
    attempts=5,
    exp_base=7,
    initial_delay=1,
    http_status_codes=[429, 500, 503, 504]
)

🤖 2. Build Your First AI Agent
2.1 Understanding AI Agents

Traditional LLM workflow:

Prompt → LLM → Response


Agent workflow:

Prompt → Thought → Action → Observation → Final Answer


Agents can think, use tools, and refine answers via feedback loops.

2.2 Agent Definition
root_agent = Agent(
    name="helpful_assistant",
    model=Gemini(
        model="gemini-2.5-flash-lite",
        retry_options=retry_config
    ),
    description="A simple agent that can answer general questions.",
    instruction="You are a helpful assistant. Use Google Search for current info or if unsure.",
    tools=[google_search],
)

2.3 Running the Agent

Create a runner:

runner = InMemoryRunner(agent=root_agent)


Run a prompt:

response = await runner.run_debug(
    "What is Agent Development Kit from Google?"
)

2.5 Try Your Own Query

Examples:

What's the weather in London?

Who won the last World Cup?

What movies are in theaters now?

The agent automatically uses Google Search when needed.

💻 3. ADK Web Interface

The ADK Web UI allows:

Interactive chat with your agent

Action/Thought traces

Agent debugging

Tool inspection

3.1 Generate a Sample Agent
!adk create sample-agent --model gemini-2.5-flash-lite --api_key $GOOGLE_API_KEY


Generated files:

sample-agent/
│── .env
│── agent.py
└── __init__.py

3.2 Start the Web UI

Get your proxy URL:

url_prefix = get_adk_proxy_url()


Start the Web UI:

!adk web --url_prefix {url_prefix}


⚠️ Never share the generated proxy URL — it contains authentication tokens.

📁 Project Structure
/
├── notebook.ipynb
├── sample-agent/
│   ├── .env
│   ├── agent.py
│   └── __init__.py
└── README.md

📚 Resources

Google ADK Documentation

Kaggle Notebooks Guide

Google AI Studio

ADK GitHub Repository

📝 License

This project is provided for learning and educational use as part of the Kaggle 5-Day Agents Program.
You may modify and reuse for personal or portfolio purposes.
