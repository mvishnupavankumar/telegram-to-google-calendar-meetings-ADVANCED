
🧠 Smart Calendar Manager for Telegram
📋 Description
This project acts as your personal assistant for managing meetings and checking your schedule directly through Telegram. It simplifies how you interact with your calendar by understanding your natural language commands. Whether you want to set up a new meeting or find out what’s on your agenda for a specific day or period, this tool handles it all without you needing to open your calendar application.

It solves the problem of juggling multiple apps to stay organized. Instead, it provides a seamless way to manage your time and commitments just by chatting with a Telegram bot, making scheduling effortless and quick.

🔍 How It Works
Trigger: The workflow starts when you send a message to your Telegram bot.

Steps:

The message is analyzed to determine if you want to create a new meeting or check existing ones.

If you want to create a meeting, an AI extracts details like meeting title, date, time, and link from your message. These details are then formatted and added to your Google Calendar.

If you want to check your schedule, the AI interprets your requested date or date range and searches your Google Calendar for events within that period.

The workflow then compiles the relevant meeting information.

End Result: You receive a confirmation message in Telegram for newly created meetings, or a list of your scheduled events for the requested period.

🚀 Use Cases
Quickly schedule meetings: Just type "Set up a meeting for Project Alpha tomorrow at 10 AM, link meet.google.com/abc" and it's added to your calendar.

Check your daily agenda: Ask "What's on my calendar for next Tuesday?" and receive a list of all your meetings for that day.

Find free slots: Inquire "Check my free time for this weekend" to get an overview of your availability.

🛠️ Setup Instructions
Import the JSON into your automation tool (like n8n).

Enter your own API keys or credentials in the appropriate fields for Google Calendar and Telegram.

Replace placeholder URLs and IDs (e.g., your_telegram_id, your_url, your_authorization, your_email@example.com) with your actual values.

Activate the workflow.

⚠️ You don’t need to code — just follow the step-by-step settings in the visual interface.

🧩 Requirements
n8n installed locally or using n8n cloud

Telegram bot token and chat ID

Google Calendar API key/OAuth2 credentials

OpenRouter/LLM API key for AI natural language processing (used for your_url and your_authorization)

🤖 Example Input JSON
JSON

{
  "name": "MEETINGS WORKFLOW TO CALENDER FROM TELEGRAM",
  "nodes": [
    {
      "type": "n8n-nodes-base.telegramTrigger",
      "parameters": {
        "updates": ["message"]
      }
    },
    {
      "parameters": {
        "method": "POST",
        "url": "your_url",
        "jsonBody": "= {\n  \"model\": \"mistralai/mistral-7b-instruct\",\n  \"messages\": [\n    {\n      \"role\": \"system\",\n      \"content\": \"You are a meeting data extractor...\"\n    },\n    {\n      "role": "user",
      "content": "{{ $json.message.text }}"
    }\n  ]\n}",
        "options": {
          "response": {
            "response": {
              "fullResponse": true,
              "neverError": true,
              "responseFormat": "json"
            }
          }
        }
      },
      "type": "n8n-nodes-base.httpRequest",
      "name": "TAKES MEETING DETAILS1"
    },
    {
      "parameters": {
        "calendar": {
          "value": "your_email@example.com"
        },
        "start": "= {{ $json.start.dateTime }}",
        "end": "= {{ $json.end.dateTime }}",
        "additionalFields": {
          "description": "=<a href=\"{{ $json.description }}\">Join Meeting</a>",
          "location": "Asia/Kolkata",
          "summary": "= {{ $json.summary }}"
        }
      },
      "type": "n8n-nodes-base.googleCalendar",
      "name": "Create an event"
    }
  ]
}
✨ Output
For meeting creation requests: A Telegram message confirming the meeting has been set, including its title, start time, and end time.

For schedule inquiry requests: A Telegram message listing events found for the specified date(s), including event title, date, time (if applicable), and a link to the event. If no events are found, it will state "NO MEETINGS FOR THE GIVEN [date/date range]".

📎 Notes
Supports natural language time and date expressions like “tomorrow 5pm,” “next Tuesday morning,” "this month," or "next 3 days."

Uses IST timezone (UTC+5:30) for all calendar operations and time conversions.

Works out of the box after replacing the token values and credential details.

