# Multi-Agent AI Workflow with n8n

## Executive Summary

A portfolio implementation of an **AI-agent orchestration workflow** using **n8n, OpenAI, and Google Workspace integrations**.

The workflow provides a conversational entry point for business requests, maintains session context, interprets intent, selects an appropriate connected tool, executes the workflow, and returns a structured response.

> **Portfolio note:** This repository demonstrates an implemented architecture and deployment pattern. Production use would require enterprise identity, authorization, audit, observability, approval controls, data-retention policies, and operational safeguards appropriate to the connected systems.

---

## Business Problem

Enterprise users often work across email, documents, calendars, spreadsheets, and web research. Moving between these systems creates fragmented workflows and repetitive manual work.

This project demonstrates a **single conversational orchestration layer** that can translate a natural-language request into an action across multiple business tools.

### Example requests

- Send an email
- Search for information
- Create or manage a document
- Create or manage a calendar event
- Record or update spreadsheet data

---

## Architecture

```text
User / Frontend
      │
      ▼
HTTPS Request
      │
      ▼
Webhook
      │
      ▼
n8n Workflow
      │
      ▼
AI Agent
 ┌────┼───────────────────────────────┐
 │    │                               │
 ▼    ▼                               ▼
OpenAI  Session Memory          Connected Tools
                                      │
                 ┌────────────────────┼────────────────────┐
                 ▼                    ▼                    ▼
               Gmail              Google Search       Google Docs
                 │                    │                    │
                 └──────────────┬─────┴──────────┬─────────┘
                                ▼                ▼
                         Google Calendar    Google Sheets
                                │
                                ▼
                         Structured Response
                                │
                                ▼
                           Frontend / User
```

### Deployment pattern

The implementation uses **Docker + n8n + Nginx + HTTPS** as the deployment pattern, with Nginx acting as the reverse proxy and TLS termination layer.

---

## AI Agent Responsibilities

The AI Agent acts as the orchestration layer between the user request and connected services.

It is responsible for:

1. Interpreting the natural-language request.
2. Using available session context.
3. Determining whether a tool is required.
4. Selecting the appropriate connected capability.
5. Passing the request to the selected tool.
6. Returning the tool result as a user-facing response.

This demonstrates the core pattern:

**Request → Intent → Orchestration → Tool Execution → Response**

---

## Connected Capabilities

| Capability | Purpose |
|---|---|
| Gmail | Email-oriented actions |
| Google Search | Information retrieval |
| Google Docs | Document-oriented actions |
| Google Calendar | Calendar actions |
| Google Sheets | Spreadsheet-oriented actions |
| Session Memory | Conversational context |
| OpenAI | AI-agent reasoning/orchestration |

---

## Request Lifecycle

1. User submits a natural-language request.
2. Frontend sends an HTTPS request to the webhook.
3. n8n receives the message and session identifier.
4. AI Agent interprets the request.
5. Session memory provides relevant conversational context.
6. The agent determines whether a connected tool is required.
7. The selected tool executes the requested operation.
8. The tool result is returned to the agent.
9. n8n returns a structured response to the frontend.

---

## Enterprise Transformation Lens

The project demonstrates how an AI interface can sit above existing business systems rather than replacing them.

```text
Business Request
      ↓
AI Interpretation
      ↓
Workflow Orchestration
      ↓
Controlled Tool Execution
      ↓
Business Outcome
```

For an enterprise production rollout, the orchestration layer should additionally address:

- Identity and role-based authorization
- Approval gates for consequential actions
- Tool-level permissions
- Audit trails
- Observability and tracing
- Retry and idempotency controls
- Failure and fallback handling
- Data retention and privacy
- Rate limiting and abuse controls
- Credential isolation and secret management

These are **production-hardening considerations**, not claims that every control is implemented in this portfolio repository.

---

## Security

Never commit the following to source control:

- API keys
- OAuth tokens
- Passwords
- Webhook secrets
- n8n credential exports
- Production environment files containing secrets

Use environment variables and managed secret storage for deployment credentials.

---

## Technology Stack

- **n8n** — workflow and agent orchestration
- **OpenAI** — language model capability
- **Google Workspace** — connected business tools
- **Docker** — containerized deployment
- **Nginx** — reverse proxy
- **HTTPS / SSL** — secure transport

---

## Portfolio Positioning

> **A conversational AI orchestration layer that connects business requests to workflow automation across enterprise tools.**

This project supports an **AI Transformation / Program Management** narrative by demonstrating practical exposure to agentic workflows, integration patterns, deployment architecture, automation, and the governance considerations required when AI can interact with business systems.
