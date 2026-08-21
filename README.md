# 🤖 Multi-Agent AI Workflow with n8n

A production-ready AI-powered automation system built using **n8n**, **OpenAI**, **Google Workspace integrations**, **Docker**, **Nginx**, and **HTTPS**.

The application receives user requests through a webhook-based interface, processes them using an AI Agent, maintains conversational context, selects the appropriate connected tool, and returns a structured response to the frontend.

---

# 🚀 Project Overview

This project demonstrates how AI Agents can orchestrate multiple external services through a single conversational interface.

Users can interact with the AI Agent and request actions such as:

- 📧 Sending emails
- 🔍 Searching for information
- 📄 Creating and managing documents
- 📅 Managing calendar events
- 📊 Recording and updating spreadsheet data

The AI Agent determines the appropriate action and interacts with the connected tools through the n8n workflow.

---

# 🏗️ Complete System Architecture

```text
                              ┌───────────────────────┐
                              │         USER          │
                              └───────────┬───────────┘
                                          │
                                          │ Message / Request
                                          ▼
                    ┌─────────────────────────────────────┐
                    │       WEB APPLICATION / FRONTEND    │
                    │                                     │
                    │       Interactive AI Chat UI        │
                    └──────────────────┬──────────────────┘
                                       │
                                       │ HTTPS POST
                                       ▼
          ┌─────────────────────────────────────────────────────┐
          │                 PRODUCTION WEBHOOK                  │
          │                                                     │
          │  /webhook/multiagent                                │
          │                                                     │
          │  Request:                                           │
          │  {                                                  │
          │    "message": "User request",                      │
          │    "sessionId": "unique-session-id"                │
          │  }                                                  │
          └───────────────────────┬─────────────────────────────┘
                                  │
                                  ▼
              ┌─────────────────────────────────────────┐
              │              NGINX                      │
              │                                         │
              │ • Reverse Proxy                         │
              │ • HTTPS / SSL                           │
              │ • Custom Domain Routing                 │
              │ • WebSocket Support                     │
              └───────────────────┬─────────────────────┘
                                  │
                                  │ localhost
                                  ▼
          ┌─────────────────────────────────────────────────┐
          │              DOCKER CONTAINER                    │
          │                                                  │
          │                     n8n                          │
          │                                                  │
          │              Internal Port: 5678                 │
          └──────────────────────┬───────────────────────────┘
                                 │
                                 ▼
          ┌─────────────────────────────────────────────────┐
          │                n8n WORKFLOW                      │
          │                                                  │
          │              Webhook Trigger                     │
          │                     │                            │
          │                     ▼                            │
          │                  AI AGENT                        │
          │                     │                            │
          └───────────────┬─────┴──────┬────────────────────┘
                          │            │
                          │            │
                          ▼            ▼
                ┌──────────────┐  ┌──────────────┐
                │ OpenAI Chat  │  │    Memory    │
                │    Model     │  │ Session Data │
                └──────────────┘  └──────────────┘
                          │
                          │ Tool Selection
                          ▼
        ┌──────────────────────────────────────────────┐
        │               CONNECTED TOOLS                 │
        │                                              │
        │  🔍 Google Search                            │
        │  📄 Google Docs                              │
        │  📅 Google Calendar                          │
        │  📊 Google Sheets                            │
        │  📧 Gmail                                    │
        └──────────────────────┬───────────────────────┘
                               │
                               ▼
                 ┌──────────────────────────┐
                 │     AI AGENT OUTPUT      │
                 │                          │
                 │ {                        │
                 │   "output": "..."        │
                 │ }                        │
                 └────────────┬─────────────┘
                              │
                              ▼
                 ┌──────────────────────────┐
                 │      WEB FRONTEND        │
                 │                          │
                 │ Interactive AI Response  │
                 └──────────────────────────┘

🧠 AI Agent Architecture
The AI Agent acts as the central orchestration layer.

                    USER MESSAGE
                         │
                         ▼
                  ┌─────────────┐
                  │   AI AGENT  │
                  └──────┬──────┘
                         │
        ┌────────────────┼────────────────┐
        │                │                │
        ▼                ▼                ▼
   LLM MODEL          MEMORY           TOOLS
        │                │                │
        │                │        ┌───────┴────────┐
        │                │        │                │
        ▼                ▼        ▼                ▼
    Reasoning       Context    Gmail        Google Search
                               Docs
                               Calendar
                               Sheets

🔄 Request Processing Flow
1. User enters a request
        ↓
2. Frontend sends HTTPS POST request
        ↓
3. n8n Webhook receives:
   - message
   - sessionId
        ↓
4. AI Agent processes the request
        ↓
5. Memory retrieves conversation context
        ↓
6. OpenAI model analyzes user intent
        ↓
7. AI Agent determines whether a tool is required
        ↓
8. Selected tool executes the requested action
        ↓
9. Tool result is returned to the AI Agent
        ↓
10. AI Agent generates the final response
        ↓
11. n8n returns structured JSON
        ↓
12. Frontend displays the AI response