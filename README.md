# AI Chat Stream Interfaces

This repository contains browser-based streaming chat interfaces for interacting with local AI servers: Ollama and LM Studio. These HTML/JavaScript applications provide real-time conversation capabilities without requiring a web server.

## Features

- **Streaming Responses**: Receive AI responses in real time as they are generated
- **Prompt Management**: Save prompts to local text files
- **Response Handling**: Copy generated answers to clipboard
- **Performance Tracking**: Display elapsed response time
- **No Server Required**: Open directly in any modern browser
- **Local AI Integration**: Connect to local Ollama or LM Studio servers

## Apps Included

### Ollama Chat Stream

A browser-based interface for interacting with a local Ollama AI server using streaming responses.

#### Setup

1. Install [Ollama](https://ollama.ai/) locally on your machine
2. Quit Ollama app from the toolbar.
3. From the cli run run the following.
    ```bash
    export OLLAMA_ORIGINS="*"
    ollama serve
    ```
4. keep the cli open for the server to corretly operate.  
5. Open Ollama Chat Stream.html in your favorite browser.  

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
