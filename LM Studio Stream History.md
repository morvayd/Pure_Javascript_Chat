# LM Studio Chat Interface with History

A browser-based chat interface for interacting with LM Studio's local language models, featuring real-time streaming responses and persistent chat history storage.

## Overview

This HTML application provides a user-friendly web interface to communicate with LM Studio's local server. It includes streaming response capabilities, automatic chat history persistence, and token usage tracking.

**Version:** 2026.04.07

## Features

### Core Functionality
- **Real-time Streaming**: Displays AI responses as they're generated
- **Local Model Integration**: Connects to LM Studio server running on localhost:1234
- **Persistent History**: Saves all conversations to a SQLite database
- **Token Tracking**: Monitors input and output token usage
- **Response Timing**: Tracks and displays response generation time

### Database Features
- **SQLite Storage**: Uses sql.js for in-browser database management
- **Auto-save**: Automatically saves conversations to selected folder
- **File System Access**: Persistent folder access using File System Access API
- **Dual Storage**: Falls back to localStorage if file access unavailable
- **Record Tracking**: Displays total conversation count

### User Interface
- **Clean Design**: Simple, responsive layout
- **Copy Function**: One-click answer copying to clipboard
- **Prompt Saving**: Export prompts as versioned text files
- **Model Display**: Shows currently loaded LM Studio model
- **Status Indicators**: Visual feedback for database and connection status

## Prerequisites

### Required Software
1. **LM Studio**: Download from [lmstudio.ai](https://lmstudio.ai)
2. **Modern Browser**: Chrome or Edge (for File System Access API)
3. **Local Model**: Any LLM model loaded in LM Studio

### LM Studio Configuration

#### Method 1: GUI Setup
1. Open LM Studio
2. Navigate to: **Developer → Local Server**
3. Click **Server Settings** (middle dropdown)
4. Enable **"Enable CORS"**
5. Start server on port **1234**

#### Method 2: CLI Setup
```bash
lms server start --cors
lms load google/gemma4-26b-a4b
```

## Installation

1. Download `LM Studio Stream History.html`
2. Open the file in Chrome or Edge browser
3. No additional installation required

## Usage

### First Time Setup

1. **Start LM Studio Server**
   - Ensure CORS is enabled
   - Verify server is running on port 1234

2. **Open the HTML File**
   - Double-click or open in browser
   - Wait for database initialization

3. **Configure Database Storage**
   - Click **"Setup Database Folder"**
   - Select a folder for persistent storage
   - Grant read/write permissions

### Using the Chat Interface

1. **Enter Your Question**
   - Type your prompt in the textarea
   - Questions can be multi-line

2. **Submit Query**
   - Click **"Submit"** button
   - Watch response stream in real-time

3. **View Results**
   - Response appears with streaming effect
   - Timing and token information displayed
   - Automatically saved to database

4. **Additional Actions**
   - **Clear**: Clears the input textarea
   - **Copy Answer**: Copies response to clipboard
   - **Save Prompt**: Downloads prompt as versioned text file

## Database Schema

The application stores conversations in a SQLite database with the following structure:

```sql
CREATE TABLE chat_history (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    date TEXT NOT NULL,
    user_id TEXT NOT NULL,
    computer_os TEXT NOT NULL,
    model TEXT NOT NULL,
    user_input TEXT NOT NULL,
    ollama_response TEXT NOT NULL,
    response_time_seconds INTEGER,
    input_tokens INTEGER,
    output_tokens INTEGER,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
)
```

### Database Files
- **Primary**: `lmstudio_chat_history.db` (in selected folder)
- **Backup**: localStorage (browser storage)
- **Handle Storage**: IndexedDB (for folder permissions)

## File Naming Convention

Saved prompts follow this pattern:
```
Prompt LM Studio Chat - YYYY.MM.DD vN.txt
```

Example: `Prompt LM Studio Chat - 2026.06.05 v0.txt`

## Technical Details

### API Endpoints Used
- `GET http://localhost:1234/v1/models` - Retrieves loaded model info
- `POST http://localhost:1234/v1/chat/completions` - Sends chat requests

### Request Format
```json
{
  "messages": [
    { "role": "system", "content": "You are a helpful assistant." },
    { "role": "user", "content": "user prompt here" }
  ],
  "model": "local-model",
  "temperature": 0.7,
  "stream": true
}
```

### Dependencies
- **sql.js**: v1.8.0 (loaded from CDN)
- **File System Access API**: Browser native
- **IndexedDB**: Browser native

## Browser Compatibility

| Feature | Chrome | Edge | Firefox | Safari |
|---------|--------|------|---------|--------|
| Core Chat | ✅ | ✅ | ✅ | ✅ |
| File System Access | ✅ | ✅ | ❌ | ❌ |
| localStorage Fallback | ✅ | ✅ | ✅ | ✅ |

**Note**: Firefox and Safari users will use localStorage instead of file system storage.

## Troubleshooting

### "Could not connect to LM Studio"
- Verify LM Studio server is running
- Check CORS is enabled
- Confirm port 1234 is not blocked
- Ensure a model is loaded

### "Database not initialized"
- Refresh the page
- Check browser console for errors
- Clear browser cache if needed

### "Folder access lost"
- Click "Setup Database Folder" again
- Re-grant folder permissions
- Previous data remains in localStorage

### No Streaming Response
- Check LM Studio server logs
- Verify model is properly loaded
- Try restarting LM Studio server

## Data Privacy

- **All data stays local**: No external servers contacted
- **User ID**: Generated locally, stored in browser
- **Database**: Stored in user-selected folder
- **No telemetry**: No usage tracking or analytics

## Limitations

- Requires LM Studio running locally
- File System Access API limited to Chrome/Edge
- Single conversation thread (no multi-chat support)
- No conversation editing or deletion UI

## Future Enhancements

Potential improvements:
- Multi-conversation management
- Export/import database functionality
- Conversation search and filtering
- Custom system prompts
- Model parameter controls
- Dark mode theme

## License

This is a standalone HTML application. Check LM Studio's license for model usage terms.

## Support

For issues related to:
- **LM Studio**: Visit [lmstudio.ai](https://lmstudio.ai)
- **This Interface**: Check browser console for error messages
- **Models**: Refer to specific model documentation

## Version History

- **v2026.04.07**: Current version with streaming and history features

---

**Note**: This application is designed for local use with LM Studio. Ensure your LM Studio server is properly configured before use.