# telegram-to-google-calendar-meetings-ADVANCED

---

## 🧩 Workflow Overview

### 1. **Telegram Trigger**
- Captures a message from the user.
- Routes it via a `Switch` node to decide the intent.

### 2. **LLM-Powered Parsing**
- If setting a meeting, the message is passed to OpenRouter's Mistral model with custom prompt engineering.
- Extracts: `MEETING`, `DATE`, `TIME`, `LINK`

### 3. **Data Cleaning and Formatting**
- JS code node splits and parses LLM output.
- Converts time to ISO format in IST timezone.
- Prepares calendar-friendly event JSON.

### 4. **Create Google Calendar Event**
- Uses Google Calendar OAuth2 to create a new event.
- Adds title, link, time, and location.

### 5. **Confirmation**
- After creating the event, sends a formatted success message to Telegram.

### 6. **Calendar Event Lookup**
- If the intent was `check` or `free`, extracts just the `DATE` using LLM and fetches all events for that day.

---

## 🛠️ Requirements

- [n8n](https://n8n.io/)
- A [Telegram Bot](https://core.telegram.org/bots#3-how-do-i-create-a-bot)
- A [Google Calendar](https://developers.google.com/calendar) OAuth2 connection
- An [OpenRouter](https://openrouter.ai) API Key
- Enable timezone as `Asia/Kolkata` (UTC+5:30)

---

## 🔐 Credentials

You need to add the following credentials in n8n:

- **Telegram API** – For Telegram Bot connection
- **Google Calendar OAuth2** – To create/fetch calendar events
- **OpenRouter API Key** – For making LLM calls to extract meeting data

---

## 🧪 Supported Commands (via Telegram)

| Command | Function |
|--------|----------|
| `set meeting with Alice tomorrow 3pm` | Creates a calendar event |
| `arrange a call with team next Friday 11am` | Creates a calendar event |
| `check meetings for 5th July` | Lists all events |
| `free on 7th July?` | Lists all events (can add free slot detection logic) |

---

## 📂 Files Included

- `n8n-workflow.json` – Full exported workflow
- `README.md` – This documentation

---

## 📸 Example Output

**Telegram User Input:**

---

## 🚀 How to Use

1. Clone this repo.
2. Import the `n8n-workflow.json` file into your n8n instance.
3. Set up credentials:
   - Telegram Bot Token
   - Google Calendar OAuth2
   - OpenRouter API Key
4. Activate workflow.
5. Start chatting with your Telegram bot!

---

## 📞 Need Help?

Create an issue in the repo or contact the creator on Telegram.

---

## ⚖️ License

MIT License – use and modify freely.
