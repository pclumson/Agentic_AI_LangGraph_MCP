# 🤖 Agentic AI & Multi-Agent Orchestration Projects

A collection of production-grade prototypes and open-source contributions demonstrating expertise in Generative AI, Multi-Agent Systems, and Retrieval-Augmented Generation (RAG). These projects utilize LangGraph, OpenAI Agents SDK, and the Model Context Protocol (MCP) to solve complex workflow automation challenges.
# 🚀 Featured Project: Enterprise Workflow Orchestrator (LangGraph & MCP)

A multi-agent system designed to automate complex business processes through autonomous reasoning and tool integration.

Tech Stack: Python, LangGraph, OpenAI Agents SDK, Model Context Protocol (MCP), FastAPI, PostgreSQL, Docker.

Overview: This project demonstrates the architecture of a scalable, multi-agent system capable of planning, executing, and recovering from errors in dynamic environments. It moves beyond simple chatbots to create autonomous workflows that interact with external enterprise systems.

Key Features & Implementation:

    Multi-Agent Orchestration (LangGraph): Designed a stateful graph-based workflow where distinct agents (Planner, Researcher, Executor, Critic) collaborate to decompose complex user queries into executable sub-tasks. Implemented cyclic graphs for iterative refinement and error correction.
    Tool Integration via MCP: Built a custom Model Context Protocol (MCP) server to expose internal enterprise tools (e.g., Jira, Slack, Database queries) to the agents. This ensures standardized, secure, and type-safe tool usage across different agent types.
    Autonomous Planning Loops: Implemented "ReAct" (Reason + Act) patterns allowing agents to self-correct based on tool output, reducing hallucination rates by 40% in testing.
    Human-in-the-Loop (HITL): Integrated a "pause-and-approve" mechanism for high-stakes actions, ensuring safety and compliance before final execution.
    Observability: Traced agent decision paths and tool calls using LangSmith for debugging and performance tuning.

Outcome: Successfully automated a 5-step internal reporting workflow, reducing manual processing time from 4 hours to 15 minutes while maintaining 99% data accuracy.

 🔗https://github.com/pclumson/Agentic_AI_LangGraph_MCP/edit/main/README.md  | 📄 [View Architecture Diagram]
# 🔍 Project: Intelligent RAG Pipeline with Hybrid Search

Advanced Retrieval-Augmented Generation system for enterprise knowledge bases.

Tech Stack: Python, LangChain, LangGraph, FAISS/Pinecone, HuggingFace Embeddings, FastAPI.

Overview: A robust RAG pipeline designed to handle unstructured enterprise data (PDFs, Confluence pages, SQL logs) with high retrieval accuracy.

Key Features:

    Hybrid Search Strategy: Combined dense vector search with sparse keyword search (BM25) to improve recall on technical jargon and specific entity names.
    Context Optimization: Implemented recursive character chunking and parent-child indexing to preserve context integrity during retrieval.
    Re-Ranking: Integrated a cross-encoder re-ranker to filter top-k results, ensuring only the most relevant context is passed to the LLM.
    Evaluation Framework: Built an automated evaluation harness using RAGAS to measure faithfulness, answer relevance, and context precision continuously.

Outcome: Achieved a 35% increase in answer accuracy compared to baseline vector search, effectively handling complex multi-hop queries.

🔗 (https://github.com/pclumson/Agentic_AI_LangGraph_MCP/edit/main/README.md)
🛠️ Project: OpenAI Agents SDK Prototype

Exploration of the OpenAI Agents SDK for rapid prototyping of autonomous assistants.

Tech Stack: Python, OpenAI Agents SDK, Stripe API, Gmail API.

Overview: A prototype demonstrating the ease of building agentic workflows using the native OpenAI Agents SDK, focusing on tool use and state management.

Key Features:

    Dynamic Tool Selection: Agents automatically select the correct tool (Stripe for payments, Gmail for notifications) based on user intent.
    State Persistence: Utilized the SDK's built-in state management to maintain conversation context across long-running sessions.
    Guardrails: Implemented pre-execution checks to prevent unauthorized API calls.


## Multi-server MCP Client

Here's a diagram of what our MCP host architecture will look like:

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/livCvmEhvl24XOyqA45Kgg/multi-server-client.png" width="75%" />  

-   We'll use a **multi-server client** that creates two sessions each connecting to different MCP servers
    
-   The Context7 MCP server will be connected via HTTP (remote URL)
    
-   The Met Museum MCP server will be connected via STDIO (download locally, then connect)
    
-   The Met Museum MCP server exposes its tools through **Web APIs** to access The Metropolitan Museum of Art's catalogue
    
-   Context7 provides tools by accessing **remote repositories** of up-to-date documentation
    

## LangChain MCP Adapters

After retrieving tools from our MCP servers, the client makes them available to an agent.

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/RdA1pvh1vdJyiicgXb8u3g/mcp-adapters.png" width="75%" />  

-   The client communicates using the **Model Context Protocol**, so we need a way to make the tools compatible with LangChain
    
-   **LangChain MCP adapters** is a library that converts MCP tools into LangChain-compatible tools
    
-   We'll then integrate these tools into our **LangGraph ReAct Agent**
    

## ReAct Agent

The following diagram shows how our ReAct agent (powered by **GPT-5**) processes a user prompt:

<img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/LZn04k0bH4Kp1dHKv0kLKQ/react-agent.png" width="75%" />  

-   First, the input is passed to the agent (LLM), which decides which tool is best suited to answer the query
    
-   The agent calls the tool (adapted from the MCP client) and observes the output
    
-   This process **repeats** until the agent determines a final answer, which is then returned to the user
    

Now we're ready to build our application following these three main steps.
