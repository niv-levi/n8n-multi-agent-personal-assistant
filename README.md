# n8n Multi-Agent Personal Assistant

A personal automation built with n8n, using one main AI agent and three specialized agents. The assistant is controlled through Telegram, so everyday tasks can be handled with a simple message instead of opening and working in several different apps.

The current version can manage calendar events, handle emails, and work with customer records stored in Google Sheets.

## What It Does

The workflow starts when a message is sent to the Telegram bot.

A Manager Agent reads the request, understands what needs to be done, and sends the task to the correct specialized agent.

The system includes three specialized agents:

### Email Agent

Handles email-related requests through Gmail.

It can:

- Create an email draft
- Send an email when the user explicitly asks for it

Example:

> Send an email to daniel@example.com and tell him the meeting was moved to tomorrow.

### Calendar Agent

Handles calendar-related requests through Google Calendar.

It can:

- Check existing events
- Create new events
- Understand relative dates such as today and tomorrow

Example:

> Create a meeting tomorrow at 10:00 called Project Review.

### Customers Agent

Handles customer records stored in Google Sheets.

It can:

- Add a new customer
- Search for an existing customer
- Return customer details
- Check for duplicate records before adding a customer

Example:

> Add Ron Levi, order 3333, for website and automation development.

Or:

> Show me all the details for Ron Levi.

## How It Works

```text
Telegram
   |
   v
Manager Agent
   |
   +--> Email Agent --> Gmail
   |
   +--> Calendar Agent --> Google Calendar
   |
   +--> Customers Agent --> Google Sheets
   |
   v
Telegram Reply
```

The Manager Agent acts as the main entry point. It does not perform the actions itself. Instead, it decides which specialized agent should handle each request.

A single message can also require more than one agent. For example:

> Create a meeting tomorrow at 10:00 and send Daniel an email about it.

The Manager Agent can delegate the calendar action to the Calendar Agent and the email action to the Email Agent.

## Key Features

- Multi-agent workflow built in n8n
- Telegram as the main user interface
- Manager Agent for routing requests
- Three specialized AI agents
- Gmail integration
- Google Calendar integration
- Google Sheets integration
- Customer duplicate checking
- Customer search by stored data
- Conversation memory for short-term context
- Natural language requests
- Automatic handling of relative dates using the Asia/Jerusalem timezone

## Example Requests

```text
What do I have on my calendar tomorrow?

Create an event tomorrow at 16:00 called Project Meeting.

Draft an email to Daniel about the new proposal.

Send an email to daniel@example.com and tell him I will call tomorrow.

Add a new customer named Ron Levi, order number 3333, for website and automation development.

Show me all the details for customer Ron Levi.
```

## Tech Stack

- n8n
- OpenAI
- Telegram Bot API
- Gmail
- Google Calendar
- Google Sheets

## Workflow Structure

The workflow is intentionally kept small and easy to follow.

The main workflow contains:

- Telegram Trigger
- Manager Agent
- Simple Memory
- Telegram Reply
- Email Agent
- Calendar Agent
- Customers Agent

Each specialized agent has access only to the tools it needs.

### Email Agent Tools

- Send Email
- Create Draft

### Calendar Agent Tools

- Get Events
- Create Event

### Customers Agent Tools

- Find Customer
- Add Customer

## Customer Data

Customer records are stored in Google Sheets with fields such as:

- Date
- Customer name
- Order number
- Category
- Service
- Status
- Priority
- Tags
- Notes

Before adding a customer, the Customers Agent checks the existing data to reduce duplicate records.

## Setup

To run the workflow, you need:

1. An n8n instance
2. An OpenAI API credential in n8n
3. A Telegram bot created with BotFather
4. Google credentials connected to n8n for:
   - Gmail
   - Google Calendar
   - Google Sheets
5. A Google Sheet for customer records

After the credentials are connected, import the workflow JSON into n8n and select your own credentials in each integration node.

## Security

API keys, OAuth tokens, and account credentials are not included in this repository.

All external services should be connected through the n8n Credentials system.

## Current Version

This version supports text commands through Telegram.

Voice messages and additional integrations may be added in future versions.

## Screenshots

Screenshots of the workflow and real Telegram examples will be added to the repository to show how the assistant works in practice.

## Project Goal

The goal of this project was to build a simple multi-agent workflow that performs useful everyday actions while keeping the architecture clear and easy to understand.

Instead of putting every capability inside one large agent, the workflow separates responsibilities between specialized agents and lets the Manager Agent choose the right one for each request.
