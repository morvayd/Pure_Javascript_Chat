# AI Chat Stream Interfaces

This repository contains browser-based streaming chat interfaces for interacting with local AI servers: Ollama and LM Studio. These HTML/JavaScript applications provide real-time conversation capabilities without requiring a web server.

## Features

- **Streaming Responses**: Receive AI responses in real time as they are generated
- **Prompt Management**: Save prompts to local text files
- **Response Handling**: Copy generated answers to clipboard
- **Performance Tracking**: Display elapsed response time
- **Database Logging**: SQLite database for persistent conversation storage (Ollama SQLite version)
- **Token Tracking**: Monitor input and output token usage (Ollama SQLite version)
- **No Server Required**: Open directly in any modern browser
- **Local AI Integration**: Connect to local Ollama or LM Studio servers

## Apps Included

### Ollama Chat Stream with SQLite

A browser-based interface for interacting with a local Ollama AI server with built-in SQLite database functionality for persistent conversation logging.

#### Features

- Real-time streaming responses from Ollama AI
- Automatic conversation logging to SQLite database
- Token tracking (input and output tokens)
- Auto-save functionality after each Q&A
- File System Access API for persistent database storage
- Automatic reconnection prompt on restart
- System detection (OS, user ID)
- Backward compatible with older database schemas

#### Database Schema

Stores conversations with: date, user_id, computer_os, model, user_input, ollama_response, response_time_seconds, input_tokens, output_tokens, and timestamp.

#### Setup

1. Install [Ollama](https://ollama.ai/) locally on your machine
2. Start the Ollama server in server mode with CORS enabled:
   ```bash
   export OLLAMA_ORIGINS="*"
   ollama serve
   ```
3. Keep the terminal open for the server to operate correctly.

#### Usage

1. Open `Ollama Chat Stream with SQLite.html` in Chrome or Edge browser
2. Click `Setup Database Folder` to create or connect to a database file
3. Select your preferred Ollama model from the dropdown
4. Enter your prompt and click `Submit`
5. Conversations are automatically saved to the database after each Q&A
6. On restart, you'll be prompted to reconnect to your database file

**Note**: Requires Chrome or Edge browser for File System Access API support.

### Ollama Chat Stream

A browser-based interface for interacting with a local Ollama AI server using streaming responses (without database functionality).

#### Setup

1. Install [Ollama](https://ollama.ai/) locally on your machine
2. Start the Ollama server in server mode with CORS enabled:
   ```bash
   export OLLAMA_ORIGINS="*"
   ollama serve
   ```
3. Keep the terminal open for the server to operate correctly.

#### Usage

1. Open `Ollama Chat Stream.html` in a modern browser
2. Wait for the page to connect to the local Ollama server at `http://localhost:11434`
3. Select the model you previously downloaded.  May dynamically change the model throughout the session.  
4. Enter your prompt in the textarea
5. Click `Submit` to start the streaming response
6. Use `Save Prompt` to save the prompt text and `Copy Answer` to copy the response

### LM Studio Stream Chat

A browser-based streaming chat interface for interacting with a local LM Studio server.

#### Setup

1. Install [LM Studio](https://lmstudio.ai/) locally
2. Start the local server on port `1234`
3. Enable CORS in the LM Studio server settings to allow browser requests

#### Usage

1. In LM Studio, select a model you previously downloaded.  
1. Open `LM Studio Stream Chat.html` in a modern browser
2. Enter your prompt in the textarea
3. Click `Submit` to start the streaming response
4. Click `Clear` to reset the prompt box
5. Use `Save Prompt` to download the prompt text and `Copy Answer` to copy the streamed response

## Requirements

- Modern web browser (Chrome, Firefox, Safari, Edge)
- Local AI server running:
  - Ollama server on `http://localhost:11434`
  - LM Studio server on `http://localhost:1234`
- CORS enabled on the respective servers for browser compatibility

## Notes

- The apps expect the servers to be running on their default ports
- If the app cannot load models from the server, a fallback list may be used
- Ensure your firewall allows local connections to the server ports
- No internet connection required beyond initial setup (if downloading models)

## Contributing

Feel free to submit issues or pull requests for improvements.

## License

MIT License