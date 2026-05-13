# Ollama Chat Stream

A browser-based interface for interacting with a local Ollama AI server using streaming responses.

## Purpose

This app provides a streaming chat interface to a local Ollama server, allowing users to:
- submit prompts from the browser
- receive streamed responses in real time
- save prompts to text files
- copy the generated answer to the clipboard
- see elapsed response time

## Installation

Starting the ollama server in server mode
1. Quit Ollama app from the toolbar
2. Enable CORS mode from Ollama.
    - (Linux / MacOS) From the cli run run the following.
        export OLLAMA_ORIGINS="*"
        ollama serve
    - (Windows) From the cli run the following.
        set OLLA<A_ORIGINS="*"
        ollama serve
3. keep the cli open for the server to corretly operate.
4. Open Ollama Chat Stream.html in your favorite browser. 

## Running the App

1. Open `Ollama Chat Stream.html` in a modern browser.
2. Wait for the page to connect to the local Ollama server.
3. Enter your prompt in the textarea.
4. Click `Submit` to start the streaming response.
5. Use `Save Prompt` to save the prompt text and `Copy Answer` to copy the response.

## Notes

- The app expects the Ollama server on `http://localhost:11434`.
- CORS must be enabled on the Ollama server for browser requests to succeed.
- If the app cannot load models, a fallback list will be used.
