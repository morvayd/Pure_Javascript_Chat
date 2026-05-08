# LM Studio Stream Chat

A browser-based streaming chat interface for interacting with a local LM Studio server.

## Purpose

This app provides a real-time streaming chat interface to LM Studio, allowing users to:
- submit prompts from the browser
- receive incremental streamed responses
- save prompts locally as text files
- copy the AI-generated answer to the clipboard
- display elapsed response time

## Installation

1. Clone or download this folder to your local machine.
2. Install LM Studio and start the local server on port `1234`.
3. Enable CORS in the LM Studio server settings so browser requests are allowed.

## Running the App

1. Open `LM Studio Stream Chat.html` in a modern browser.
2. Enter your prompt in the textarea.
3. Click `Submit` to start the streaming response.
4. Click `Clear` to reset the prompt box.
5. Use `Save Prompt` to download the prompt text, and `Copy Answer` to copy the streamed response.

## Notes

- The page expects LM Studio at `http://localhost:1234`.
- If requests fail, verify the server is running and that CORS is enabled.
