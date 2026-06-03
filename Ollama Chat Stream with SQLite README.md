# Ollama Chat Stream with SQLite

A pure JavaScript/HTML chat interface for Ollama AI with built-in SQLite database functionality for persistent conversation storage.

## Features

- 🤖 **Streaming Chat Interface** - Real-time streaming responses from Ollama AI
- 💾 **SQLite Database** - Automatic conversation logging to SQLite database
- 📊 **Token Tracking** - Records input and output token counts
- 🔄 **Auto-Save** - Automatically saves conversations after each Q&A
- 📁 **File Persistence** - Save database to disk with File System Access API
- 🔌 **Reconnection Prompt** - Automatically prompts to reconnect to your database on restart
- 🖥️ **System Detection** - Auto-detects and logs computer OS
- 👤 **User Tracking** - Generates and maintains persistent user ID

## Database Schema

Each conversation is stored with the following information:

| Column | Type | Description |
|--------|------|-------------|
| `id` | INTEGER | Auto-incrementing primary key |
| `date` | TEXT | Date in YYYY-MM-DD format |
| `user_id` | TEXT | Unique user identifier (auto-generated) |
| `computer_os` | TEXT | Operating system (Windows, macOS, Linux, etc.) |
| `model` | TEXT | Ollama model used (e.g., qwen3.5:35b) |
| `user_input` | TEXT | User's question/prompt |
| `ollama_response` | TEXT | AI's complete response |
| `response_time_seconds` | INTEGER | Time taken to generate response |
| `input_tokens` | INTEGER | Number of input tokens processed |
| `output_tokens` | INTEGER | Number of output tokens generated |
| `created_at` | DATETIME | Timestamp of conversation |

## Prerequisites

1. **Ollama Server** - Must be running locally
2. **Browser** - Chrome or Edge (for File System Access API support)
3. **CORS Enabled** - Ollama must allow browser connections

## Setup Instructions

### 1. Start Ollama Server

```bash
# Quit Ollama app from the toolbar first
export OLLAMA_ORIGINS="*"
ollama serve
```

Keep the terminal open while using the chat interface.

### 2. Open the HTML File

Open `Ollama Chat Stream with SQLite.html` in Chrome or Edge browser.

### 3. Setup Database (First Time)

1. Click the **"Setup Database Folder"** button
2. Choose a location and filename (default: `ollama_chat_history.db`)
3. If the file exists, it will load existing data
4. If the file is new, it will create a fresh database

### 4. Start Chatting

- Select your preferred Ollama model from the dropdown
- Type your question in the text area
- Click **Submit**
- Watch the response stream in real-time
- Conversation is automatically saved to the database

## Usage Workflow

### First Session
1. Open the HTML file
2. Click "Setup Database Folder"
3. Create/select your database file
4. Start chatting - all conversations are auto-saved

### Subsequent Sessions
1. Open the HTML file
2. See reconnection prompt: "Would you like to reconnect to [filename]?"
3. Click **OK** and select your database file
4. All previous conversations are loaded
5. New conversations are appended to existing data

## Features Explained

### Auto-Save
After each Q&A interaction, the database is automatically saved to disk. You'll see a status message showing:
- Database filename
- Total number of records
- Auto-save confirmation

### Token Tracking
The interface captures and displays:
- **Input tokens**: Number of tokens in your prompt
- **Output tokens**: Number of tokens in the AI's response
- Displayed in the UI: "Time to answer: 5 seconds | Input tokens: 150 | Output tokens: 300"

### Backward Compatibility
If you load an old database created before token tracking was added, the system automatically:
- Detects missing columns
- Adds `input_tokens` and `output_tokens` columns
- Preserves all existing data

### Database Status Indicator
Color-coded status bar shows:
- 🟢 **Green**: Database connected and auto-saving
- 🟡 **Yellow**: Working in memory only (click "Setup Database Folder")
- 🔴 **Red**: Error occurred

## Buttons

| Button | Function |
|--------|----------|
| **Submit** | Send your question to Ollama |
| **Clear** | Clear the input text area |
| **Setup Database Folder** | Connect to database file (create new or load existing) |
| **Save Prompt** | Export current prompt as text file |
| **Copy Answer** | Copy AI response to clipboard |

## Database Operations

### Viewing Your Data
Use any SQLite viewer to open your `.db` file:
- [DB Browser for SQLite](https://sqlitebrowser.org/) (Free, cross-platform)
- [SQLite Viewer](https://inloop.github.io/sqlite-viewer/) (Web-based)
- Command line: `sqlite3 ollama_chat_history.db`

### Example Queries

```sql
-- View all conversations
SELECT * FROM chat_history ORDER BY created_at DESC;

-- Count conversations by model
SELECT model, COUNT(*) as count 
FROM chat_history 
GROUP BY model;

-- Calculate average tokens per conversation
SELECT 
    AVG(input_tokens) as avg_input,
    AVG(output_tokens) as avg_output
FROM chat_history;

-- Find conversations from a specific date
SELECT * FROM chat_history 
WHERE date = '2026-06-03';

-- Total tokens used
SELECT 
    SUM(input_tokens) as total_input_tokens,
    SUM(output_tokens) as total_output_tokens
FROM chat_history;
```

## Technical Details

### Technologies Used
- **sql.js** - JavaScript port of SQLite (runs in browser via WebAssembly)
- **File System Access API** - Modern browser API for file operations
- **Ollama API** - Local AI model server with streaming support

### Browser Compatibility
- ✅ Chrome 86+
- ✅ Edge 86+
- ❌ Firefox (File System Access API not supported)
- ❌ Safari (File System Access API not supported)

### Data Storage
- **Primary**: SQLite database file on disk
- **Backup**: localStorage (automatic fallback)
- **Session**: In-memory database (if file access not configured)

## Troubleshooting

### "Database not initialized" Error
- Wait 1-2 seconds after page load before clicking buttons
- Refresh the page if sql.js fails to load from CDN

### "Table has no column named input_tokens" Error
- This should auto-fix on page load
- If persists, refresh the page to trigger column migration

### File Handle Lost After Restart
- This is normal browser security behavior
- Click OK on the reconnection prompt
- Select your database file again

### CORS Error
- Ensure Ollama server is started with `OLLAMA_ORIGINS="*"`
- Restart Ollama server if needed

### Model List Not Loading
- Check that Ollama server is running
- Fallback model list will be used if server unavailable

## Privacy & Security

- ✅ All data stays local (no cloud uploads)
- ✅ Database stored on your computer
- ✅ User ID is randomly generated and stored locally
- ✅ No external tracking or analytics

## Version History

**v2026.06.03**
- Added SQLite database integration
- Added token tracking (input/output)
- Added auto-save functionality
- Added reconnection prompt
- Added backward compatibility for old databases

## License

This is a standalone HTML file - feel free to modify and use as needed.

## Support

For Ollama-related issues, visit: https://ollama.ai/
For browser compatibility, ensure you're using Chrome or Edge.