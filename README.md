# Calendar Agent (n8n Workflow)

An AI-powered calendar assistant that manages Google Calendar through natural language conversations via webhook.

## What It Does

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  POST Request   │────▶│    AI Agent     │────▶│    Response     │
│   (Webhook)     │     │  (Gemini LLM)   │     │   (Webhook)     │
└─────────────────┘     └────────┬────────┘     └─────────────────┘
                                 │
                    ┌────────────┼────────────┐
                    ▼            ▼            ▼
            ┌───────────┐ ┌───────────┐ ┌───────────┐
            │  Memory   │ │   List    │ │    Add    │
            │  Buffer   │ │  Events   │ │   Event   │
            └───────────┘ └───────────┘ └───────────┘
```

## Features

- Natural language calendar queries ("What meetings do I have today?")
- Smart event scheduling ("Schedule a call with Sarah tomorrow at 3pm")
- Session-based conversation memory for context retention
- Relative date parsing (today, tomorrow, next Monday)
- Human-readable time formatting in responses
- RESTful webhook API for easy integration

## Tech Stack

| Tool | Purpose |
|------|---------|
| n8n | Workflow automation platform |
| Google Gemini | AI language model for natural language understanding |
| Google Calendar | Calendar storage and management |
| Webhook | HTTP API endpoint for external access |

## Quick Start

1. **Import** the `Calendar Agent - Webhook.json` file into your n8n instance
2. **Configure credentials** (see below)
3. **Activate** the workflow
4. **Send POST requests** to `/webhook/calendar-agent`

### Example Request

```bash
curl -X POST https://your-n8n-instance/webhook/calendar-agent \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Do I have any meetings tomorrow?",
    "sessionId": "user-123"
  }'
```

### Example Response

```
You have 2 meetings tomorrow:
- Team Standup from 9:00 AM to 9:30 AM
- Project Review from 2:00 PM to 3:00 PM
```

## Credentials Required

| Credential | How to Get |
|------------|------------|
| Google Gemini API | Get API key from [Google AI Studio](https://aistudio.google.com/app/apikey) |
| Google Calendar OAuth2 | Create OAuth2 credentials in [Google Cloud Console](https://console.cloud.google.com/apis/credentials) with Calendar API enabled |

## Workflow Nodes

| Node | Type | Purpose |
|------|------|---------|
| Webhook | Trigger | Receives POST requests with `message` and `sessionId` |
| AI Agent | LangChain | Processes natural language and orchestrates tools |
| Google Gemini Chat Model | LLM | Powers the AI reasoning and responses |
| Simple Memory | Memory | Maintains conversation context per session |
| List Events | Tool | Queries Google Calendar for events |
| Add Event | Tool | Creates new calendar events |
| Respond to Webhook | Output | Returns AI response to the caller |

## Customization

### Change the Calendar
Update the `calendar` parameter in both **List Events** and **Add Event** nodes to your calendar email.

### Modify AI Behavior
Edit the system prompt in the **AI Agent** node to customize:
- Response tone and style
- Date/time handling rules
- Supported commands

### Switch LLM Provider
Replace the **Google Gemini Chat Model** with:
- OpenAI GPT-4
- Anthropic Claude
- Any LangChain-compatible model

### Adjust Memory
Modify the **Simple Memory** node settings to change:
- Memory window size
- Session key source

## API Reference

### Endpoint
```
POST /webhook/calendar-agent
```

### Request Body
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| message | string | Yes | Natural language query or command |
| sessionId | string | Yes | Unique session identifier for memory |

### Response
Plain text response from the AI agent.

## License

MIT - Use it, modify it, share it.

## Author

**Amna Mahmood**
- [LinkedIn](https://www.linkedin.com/in/amna-mahmood-70715149/)
- [Twitter](https://x.com/AmahmoodMahmood)

---

Star this repo if you found it useful!
