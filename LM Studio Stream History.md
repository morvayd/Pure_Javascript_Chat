# LM Studio Stream History Chat Application

## Overview

A web-based chat interface for LM Studio that provides real-time streaming responses with persistent chat history storage. The application automatically saves all conversations to a SQLite database and provides conversation context from recent chat history.

## Features

### 🚀 Core Functionality

- **Real-time Streaming Responses**: Watch AI responses appear word-by-word as they're generated
- **Persistent Database Storage**: All conversations automatically saved to disk-based SQLite database
- **Conversation Context**: Automatically includes last 5 conversations as context for continuity
- **Token Usage Tracking**: Displays prompt and completion token counts for each response
- **Response Time Monitoring**: Shows time taken to complete each response
- **Model Information Display**: Shows currently loaded LM Studio model

### 💾 Database Features

- **Disk-Only Storage**: No in-memory database - all data persists on disk
- **Append-Only Operations**: Never erases or replaces existing data
- **Auto-Save**: Automatically saves after each LM Studio response
- **Data Preservation**: Maintains all records across app restarts and browser refreshes
- **Folder Selection**: Choose where to store your chat history database

### 📊 Data Stored Per Conversation

Each chat interaction saves:
- Date (YYYY-MM-DD format)
- User ID (auto-generated, persistent)
- Computer OS
- Model name
- User input (prompt)
- AI response
- Combined history (formatted conversation)
- Response time (seconds)
- Input tokens count
- Output tokens count
- Timestamp (auto-generated)

## Setup Instructions

### Prerequisites

1. **LM Studio** installed and running
2. **Modern web browser** (Chrome, Edge, or Firefox with File System Access API support)
3. **CORS enabled** in LM Studio

### Starting LM Studio Server

#### Option 1: GUI Method
1. Open LM Studio
2. Navigate to: **Developer → Local Server**
3. Click **Server Settings** (middle dropdown)
4. Enable **"Enable CORS"**
5. Start server on port **1234**

#### Option 2: CLI Method
```bash
lms server start --cors
lms load google/gemma4-26b-a4b --context-length 128000
```

### First-Time Setup

1. Open `LM Studio Stream History.html` in your web browser
2. Click the green **"Setup Database Folder"** button
3. Select a folder where you want to store your chat history
4. The app will create `lmstudio_chat_history.db` in that folder
5. Your folder selection is remembered for future sessions

## Usage

### Basic Chat Flow

1. **Enter your question** in the text area
2. Click **"Submit"** button
3. Watch the response stream in real-time
4. Response is automatically saved to database
5. Last 5 conversations are loaded as context for next query

### Button Functions

- **Submit**: Send your question to LM Studio
- **Clear**: Clear the input text area
- **Save Prompt**: Download your prompt as a text file
- **Copy Answer**: Copy the AI response to clipboard
- **Setup Database Folder**: Select/change database storage location

### Database Status Indicators

The status bar shows:
- **Warning (Yellow)**: Folder not selected - data won't be saved
- **Success (Green)**: Connected and auto-saving
- **Error (Red)**: Database error occurred

Status displays:
- Connection status
- Total record count
- Auto-save status

## Database Schema

### Table: `chat_history`

| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER | Primary key (auto-increment) |
| date | TEXT | Date of conversation (YYYY-MM-DD) |
| user_id | TEXT | Unique user identifier |
| computer_os | TEXT | Operating system |
| model | TEXT | LM Studio model name |
| user_input | TEXT | User's prompt/question |
| ollama_response | TEXT | AI's response |
| combo_history | TEXT | Formatted conversation |
| response_time_seconds | INTEGER | Time to complete response |
| input_tokens | INTEGER | Prompt token count |
| output_tokens | INTEGER | Completion token count |
| created_at | DATETIME | Timestamp (auto-generated) |

## Technical Details

### Database Operations

**Initialization**:
- Checks if database file exists
- Only creates new database if file doesn't exist
- Loads existing database and displays record count
- Never overwrites existing data on startup

**Saving Records**:
1. Load existing database from disk
2. Insert new record (append only)
3. Write complete database back to disk
4. Close database connection
5. Update status with new record count

**Loading Context**:
- Executes: `SELECT user_input, ollama_response FROM chat_history ORDER BY id DESC LIMIT 5`
- Reverses results to chronological order
- Formats as conversation history
- Includes in system message for AI context

### API Endpoints Used

- `GET http://localhost:1234/v1/models` - Get current model info
- `POST http://localhost:1234/v1/chat/completions` - Send chat requests (streaming and non-streaming)

### Browser Storage

- **IndexedDB**: Stores folder handle for persistent folder access
- **LocalStorage**: Stores user ID and prompt version tracking

## File Management

### Prompt Files

When you click "Save Prompt", the app creates a text file:
- Format: `Prompt LM Studio Chat - YYYY.MM.DD vN.txt`
- Auto-increments version number for same-day saves
- Includes: Title, Model, Date, and Prompt text

### Database File

- Filename: `lmstudio_chat_history.db`
- Location: User-selected folder
- Format: SQLite3 database
- Can be opened with any SQLite browser/tool

## Troubleshooting

### "Could not connect to LM Studio"
- Ensure LM Studio server is running on port 1234
- Verify CORS is enabled in LM Studio settings
- Check that a model is loaded

### "Database not configured"
- Click "Setup Database Folder" button
- Select a folder with write permissions
- Browser will remember your selection

### Data Not Saving
- Check database status indicator (should be green)
- Verify folder permissions
- Try selecting folder again with "Setup Database Folder"

### Browser Compatibility
- Chrome/Edge: Full support
- Firefox: Limited File System Access API support
- Safari: Not supported (no File System Access API)

## Privacy & Security

- All data stored locally on your computer
- No cloud services or external connections (except LM Studio)
- User ID generated locally and stored in browser
- Database file remains in your selected folder
- No telemetry or tracking

## Version Information

- **Application**: LM Studio Chat v2026.04.07
- **Database Schema**: v1.0
- **SQL.js Version**: 1.8.0

## Advanced Usage

### Querying the Database

You can use any SQLite tool to query your chat history:

```sql
-- Get all conversations
SELECT * FROM chat_history ORDER BY created_at DESC;

-- Get last 10 conversations
SELECT user_input, ollama_response, response_time_seconds 
FROM chat_history 
ORDER BY id DESC 
LIMIT 10;

-- Get conversations by date
SELECT * FROM chat_history 
WHERE date = '2026-06-09';

-- Get average response time
SELECT AVG(response_time_seconds) as avg_time 
FROM chat_history;

-- Get token usage statistics
SELECT 
    SUM(input_tokens) as total_input,
    SUM(output_tokens) as total_output,
    AVG(input_tokens) as avg_input,
    AVG(output_tokens) as avg_output
FROM chat_history;
```

## Support

For issues or questions:
1. Check LM Studio is running and CORS is enabled
2. Verify browser compatibility
3. Check console for error messages (F12 Developer Tools)
4. Ensure database folder has write permissions

## License

This application is provided as-is for use with LM Studio.