# Agent Stan: AI Executive Assistant

Agent Stan is an AI agent that performs the duties of a personal executive assistant. It is designed to be a powerful, flexible, and extremely low-cost alternative to commercial automation tools, reducing monthly software costs by over 95% ($30+/month --> $1/month for every 1000 emails sent).

The system operates through a conversational interface on Telegram and is capable of handling complex, multi-step tasks involving email and calendar management across multiple accounts. All requests are processed with an average response time of ~2.5 seconds.

---

### Table of Contents

- [Features](#features)
- [General Setup](#general-setup)
  - [Prerequisites](#prerequisites)
  - [Credentials](#credentials)
  - [Configuration](#configuration)
- [Running the Assistant](#running-the-assistant)
  - [Import Workflows](#import-workflows)
  - [Activate the Agent](#activate-the-agent)
  - [Using the Assistant](#using-the-assistant)
- [Advanced Customization](#advanced-customization)

---

### Features

- **Multi-Account Email & Calendar Management:** Seamlessly manage, send, and reply to emails from multiple personal and work accounts within the same conversation.
- **Simultaneous Task Execution:** Draft an email, check calendar availability across multiple accounts, and generate a Google Calendar invite all from a single natural language command.
- **High-Speed Performance:** Initial command processing and draft generation occurs in ~2.5 seconds, with final execution after approval taking an additional ~2.5 seconds.
- **Voice & Text Commands:** Interact with the agent using either text messages or voice notes, with automatic transcription powered by OpenAI's Whisper model.
- **Human-in-the-Loop Approval:** For enhanced safety and control, all outgoing actions (emails, calendar invites) are first sent to the user for a final "Approve" or "Disapprove" confirmation.
- **Intelligent Tool Use:** The system uses a **Langchain Agent** as its core "brain," allowing it to reason about a user's request, choose the correct tool (e.g., `Send_Personal_Email`, `Check_Availability`), and execute it with the appropriate parameters.

### General Setup

#### Prerequisites

1.  **Automation Platform:** This project is built on a visual workflow automation platform that can import the provided `.json` files.
2.  **API Accounts:** You will need active accounts and API keys for:
    - OpenAI
    - Google (for Gmail and Google Calendar APIs)
    - Telegram (for the bot interface)

#### Credentials

To run the agent, you must first configure the necessary credentials within your automation platform's credential manager.

1.  **OpenAI API Key:** `export OPENAI_API_KEY=...`
2.  **Google OAuth:**
    - Enable the **Gmail API** and **Google Calendar API** in your Google Cloud Console.
    - Authorize credentials for a desktop application. For personal Gmail accounts, you must add your email as a "Test user" under the "OAuth consent screen" to prevent verification errors.
    - Add the generated OAuth credentials to your platform for both a "Personal" and "School/Work" account.
3.  **Telegram Bot Token:**
    - Create a new bot using the `BotFather` on Telegram to get a token.
    - Add this token to your platform's credentials.

#### Configuration

Key parts of the agent's behavior are configured directly within the workflow nodes.

- **Agent Persona & Prompts:** The core system prompt for the AI Agent is located in the `AI Agent` node within the `Agent_Stan.json` workflow. Here you can define the agent's name, background, personality, and response preferences.
- **User Details:** Update the calendar nodes in `Create_Gcal_Invite.json` and `Check_Availability.json` with your specific email addresses.
- **Chat ID:** In the `Send Draft for Approval` nodes across various workflows, set the `Chat ID` parameter to your personal Telegram Chat ID to ensure approval requests are sent to you.

### Running the Assistant

#### Import Workflows

1.  Download all `.json` files from this repository.
2.  Import each file into your automation platform. This will create a canvas of pre-configured workflows. The connections between workflows should be preserved.

#### Activate the Agent

- The primary workflow that listens for user input is `Agent_Stan.json`. This is the only workflow that needs to be **activated**.
- All other workflows (`Send_School`, `Reply_Personal`, etc.) are designed to be executed as sub-routines by the main agent.

#### Using the Assistant

- **Initiate:** Open a chat with your Telegram bot.
- **Interact:** Send commands via text or voice notes.
  - *Example 1:* "From my school account, email jane.doe@example.com with the subject 'Coffee Chat' and ask if she's free to meet next week. Also, send her my availability."
  - *Example 2 (Voice Note):* "Reply to the last email from John and tell him that time works perfectly, I just sent him a calendar invite."
- **Approve Actions:** The agent will send you a confirmation message with the drafted email or a summary of the calendar event. Click "Approve" to execute the action.

### Advanced Customization

The modular design allows for easy modification of the core logic by editing the code within specific nodes.

- **Triage & Routing Logic:** To change how the agent handles different types of incoming messages (text, audio, attachments), edit the `Switch` nodes in `Agent_Stan.json`.
- **Calendar Availability Logic:** The JavaScript code for calculating available time slots can be found in the `Code` node within `Check_Availability.json`.
- **Email Construction Logic:** To modify how MIME messages are constructed for emails and replies, edit the `Reconstruct Email Data` nodes in `Agent_Stan.json`.
