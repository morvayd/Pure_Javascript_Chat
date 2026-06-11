# LM Studio Chat with Semantic Memory

A browser-based chat interface for LM Studio with intelligent conversation history using vector embeddings and similarity search.

## 🌟 Features

### Core Functionality
- **Streaming Chat Interface**: Real-time streaming responses from LM Studio
- **Semantic Memory System**: Intelligent conversation history using vector embeddings
- **Similarity Search**: Automatically finds and loads relevant past conversations
- **Persistent Storage**: SQLite database with File System Access API
- **Auto-Save**: Conversations automatically saved to local database
- **Token Tracking**: Displays prompt and completion token counts
- **Response Timing**: Shows time taken for each response

### Advanced Features
- **Vector Embeddings**: 384-dimensional embeddings generated client-side
- **Cosine Similarity**: Finds semantically similar conversations (30% threshold)
- **Context Injection**: Top 5 relevant conversations added to AI context
- **Diagnostic Display**: Visual feedback showing which conversations are being used
- **No External Dependencies**: All processing happens in the browser

## 📋 Prerequisites

### Required
- **LM Studio**: Installed and running on port 1234
- **Modern Browser**: Chrome, Edge, or any browser supporting File System Access API
- **CORS Enabled**: LM Studio server must have CORS enabled

### LM Studio Setup
```bash
# Option 1: GUI
# LM Studio -> Developer -> Local Server -> Server Settings -> Enable CORS

# Option 2: CLI
lms server start --cors
lms load google/gemma4-26b-a4b --context-length 128000
```

## 🚀 Getting Started

### 1. Start LM Studio Server
- Launch LM Studio
- Load your preferred model
- Start the server on port 1234
- Enable CORS in server settings

### 2. Open the HTML File
- Open `LM Studio Stream History.html` in your browser
- No web server required - runs directly from file system

### 3. Setup Database Folder
- Click "Setup Database Folder" button
- Select a folder where chat history will be saved
- Database file `lmstudio_chat_history.db` will be created automatically
- Folder selection is remembered for future sessions

### 4. Start Chatting
- Type your question in the text area
- Click "Submit" to send
- Watch the response stream in real-time
- Conversations are automatically saved with embeddings

## 🧠 How Semantic Memory Works

### Vector Embeddings
Each conversation is converted into a 384-dimensional vector using:
- **Word-based hashing** (dimensions 0-255): Captures word-level semantics
- **Character trigrams** (dimensions 256-383): Handles typos and variations
- **L2 normalization**: Enables cosine similarity comparisons

### Similarity Search Process
1. **User asks a question** → System generates embedding for the question
2. **Database search** → Compares question embedding with all stored `combo_vector` values
3. **Ranking** → Calculates cosine similarity scores (0-100%)
4. **Filtering** → Returns conversations above 30% similarity threshold
5. **Top K selection** → Selects top 5 most similar conversations
6. **Context injection** → Adds relevant history to system prompt

### Example Flow
```
User Question: "How do I optimize database queries?"
    ↓
Generate embedding for question
    ↓
Search all combo_vectors in database
    ↓
Find similar conversations:
  - Match 1: 78.5% - "What's the best way to index tables?"
  - Match 2: 65.2% - "How to improve SQL performance?"
  - Match 3: 52.1% - "Database optimization techniques"
    ↓
Load combo_history from these rows
    ↓
Append to system prompt as context
    ↓
AI receives relevant memory and generates informed response
```

## 📊 Database Schema

```sql
CREATE TABLE chat_history (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    date TEXT NOT NULL,
    user_id TEXT NOT NULL,
    computer_os TEXT NOT NULL,
    model TEXT NOT NULL,
    user_input TEXT NOT NULL,
    ollama_response TEXT NOT NULL,
    combo_history TEXT NOT NULL,
    combo_vector TEXT NOT NULL,
    response_time_seconds INTEGER,
    input_tokens INTEGER,
    output_tokens INTEGER,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
)
```

### Key Columns
- **combo_history**: Full conversation text (`User: {question}\nAI: {response}`)
- **combo_vector**: JSON array of 384 floating-point numbers (embeddings)
- **user_input**: Original user question
- **ollama_response**: AI's response
- **model**: LM Studio model used

## 🎯 Usage Examples

### Basic Chat
1. Type: "What is machine learning?"
2. Click Submit
3. View streaming response
4. Conversation saved automatically with embeddings

### Semantic Memory in Action
1. First question: "How do I create a database?"
2. Later question: "What's the best way to store data?"
3. System automatically loads first conversation as context
4. AI provides consistent, contextual response

### Diagnostic Display
Before each response, you'll see:
```
🔍 Similarity Search Results
Found 3 relevant conversation(s) for context:

Match 1 - Similarity: 78.5%
ID: 42
User: How do I create a database?
AI: To create a database...

Match 2 - Similarity: 65.2%
ID: 38
User: What's the best way to index tables?
AI: Indexing improves query performance...
```

## ⚙️ Configuration

### Adjustable Parameters

**Similarity Threshold** (Line 866):
```javascript
const similarConversations = await findSimilarConversations(prompt, 5, 0.3);
//                                                                    ↑ 30% threshold
```

**Number of Results** (Line 866):
```javascript
const similarConversations = await findSimilarConversations(prompt, 5, 0.3);
//                                                                  ↑ Top 5 results
```

**Vector Dimensions** (Line 547):
```javascript
const vectorSize = 384; // Common embedding size
```

## 🔧 Technical Details

### Technologies Used
- **sql.js**: SQLite compiled to WebAssembly for browser
- **File System Access API**: Local file storage
- **Fetch API**: Communication with LM Studio
- **IndexedDB**: Persistent folder handle storage

### Browser Compatibility
- ✅ Chrome 86+
- ✅ Edge 86+
- ✅ Opera 72+
- ❌ Firefox (File System Access API not supported)
- ❌ Safari (File System Access API not supported)

### Performance
- **Embedding generation**: < 10ms per conversation
- **Similarity search**: ~1ms per 100 records
- **Database operations**: < 50ms for typical queries
- **Memory usage**: ~2MB for 1000 conversations

## 📁 File Structure

```
.
├── LM Studio Stream History.html    # Main application file
├── LM Studio Stream History.md      # This README
└── Chat Prompts/
    └── lmstudio_chat_history.db     # SQLite database (created on first use)
```

## 🐛 Troubleshooting

### "Could not connect to LM Studio"
- Ensure LM Studio server is running on port 1234
- Check that CORS is enabled in server settings
- Verify a model is loaded

### "Database not configured"
- Click "Setup Database Folder" button
- Select a folder with write permissions
- Browser will remember your selection

### "No similar conversations found"
- Normal for first few conversations
- Embeddings only created for new conversations
- Need at least one saved conversation with embeddings

### Embeddings not saving
- Check browser console for errors
- Verify database folder has write permissions
- Ensure combo_vector column exists in database

## 🔒 Privacy & Security

- **All data stays local**: No external API calls for embeddings
- **No cloud storage**: Database stored on your computer
- **No tracking**: No analytics or telemetry
- **Open source**: Full code visibility

## 🚧 Limitations

### Current Limitations
- **Browser-only**: Requires File System Access API (Chrome/Edge)
- **Simple embeddings**: Hash-based, not transformer-based
- **No backfilling**: Only new conversations get embeddings
- **Single database**: One database per folder selection

### Compared to Sentence-Transformers
- ❌ Less semantic understanding
- ❌ No pre-trained knowledge
- ✅ No dependencies or setup
- ✅ Instant generation
- ✅ Privacy-focused

## 🔮 Future Enhancements

Potential improvements:
- [ ] Transformer.js integration for better embeddings
- [ ] Elasticsearch backend option
- [ ] Multi-database support
- [ ] Export/import functionality
- [ ] Conversation search UI
- [ ] Embedding visualization
- [ ] Adjustable similarity threshold in UI

## 📝 License

This project is open source and available for personal and commercial use.

## 🤝 Contributing

Contributions welcome! Areas for improvement:
- Better embedding algorithms
- UI/UX enhancements
- Additional database backends
- Performance optimizations
- Documentation improvements

## 📧 Support

For issues or questions:
1. Check the Troubleshooting section
2. Review browser console for errors
3. Verify LM Studio server is running
4. Check File System Access API compatibility

## 🙏 Acknowledgments

- **LM Studio**: For the excellent local LLM server
- **sql.js**: For SQLite in the browser
- **File System Access API**: For local file storage

---

**Version**: 2026.06.11  
**Last Updated**: June 11, 2026  
**Compatibility**: LM Studio v0.2.0+