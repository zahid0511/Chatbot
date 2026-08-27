# 🤖 Gemini CLI Assistant

> **A lightweight Python command-line AI assistant powered by Google's Gemini API.**

A minimal, secure-by-design command-line application that accepts a user's question, sends it to **Gemini 3.5 Flash**, and returns the generated response directly in the terminal.

The project demonstrates a clean separation between **application logic** and **configuration/secrets management**, while keeping the implementation intentionally simple and easy to understand.

---

## ✨ Features

* 💬 Ask questions directly from the terminal
* ⚡ Powered by **Gemini 3.5 Flash**
* 🔐 API key loaded from environment variables
* 🧩 Separate configuration module for secret management
* 🐍 Built entirely with Python
* 📦 Minimal dependencies
* 🛠️ Easy to extend into a larger AI application

---

## 🏗️ Architecture

```text
┌──────────────────────┐
│      User Input      │
│   Terminal / CLI     │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│      app.py          │
│                      │
│   talk(question)     │
└──────────┬───────────┘
           │
           │ getthekey()
           ▼
┌──────────────────────┐
│     config.py        │
│                      │
│  Load GOOGLE_API_KEY │
│      from .env        │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│    Google GenAI      │
│        Client        │
└──────────┬───────────┘
           │
           │ generate_content()
           ▼
┌──────────────────────┐
│   Gemini 3.5 Flash   │
│      AI Model        │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│   Generated Answer   │
│      Terminal        │
└──────────────────────┘
```

### Request Flow

```text
Question
   ↓
talk(question)
   ↓
Load API Key
   ↓
Initialize GenAI Client
   ↓
Send Prompt
   ↓
Gemini 3.5 Flash
   ↓
Receive Response
   ↓
Print response.text
```

---

## 📁 Project Structure

```text
gemini-cli-assistant/
│
├── app.py
├── config.py
├── .env
├── .gitignore
├── requirements.txt
└── README.md
```

### File Responsibilities

| File               | Responsibility                                      |
| ------------------ | --------------------------------------------------- |
| `app.py`           | CLI interaction and Gemini API communication        |
| `config.py`        | Environment configuration and API-key retrieval     |
| `.env`             | Stores the local Google API key                     |
| `.gitignore`       | Prevents sensitive/local files from being committed |
| `requirements.txt` | Python dependency management                        |
| `README.md`        | Project documentation                               |

---

## 🧠 How It Works

The application follows a simple request-response pipeline.

### 1. Capture user input

The application starts by asking the user for a question:

```python
qs = input("Ask any question")
```

### 2. Pass the question to the AI layer

The question is passed to the `talk()` function:

```python
info = talk(qs)
```

### 3. Retrieve the API key

The application obtains the Google API key through the configuration module:

```python
key = getthekey()
```

This keeps configuration access separate from the core application logic.

### 4. Initialize the GenAI client

The Google GenAI client is initialized using the retrieved API key:

```python
obj = genai.Client(api_key=key)
```

### 5. Generate the response

The user's question is sent to Gemini:

```python
response = obj.models.generate_content(
    model="gemini-3.5-flash",
    contents=question
)
```

### 6. Display the answer

The generated text is printed back to the terminal:

```python
print(info.text)
```

---

## 🔐 Environment & API Key Management

The API key is **not hard-coded inside the application**.

Instead, the configuration module loads environment variables using `python-dotenv`:

```python
load_dotenv(find_dotenv(), override=True)
```

The key is then retrieved with:

```python
os.getenv("GOOGLE_API_KEY")
```

This approach keeps credentials outside the source code and makes the application easier to configure across different environments.

### `.env`

Create a `.env` file in the project root:

```env
GOOGLE_API_KEY=your_google_api_key_here
```

> ⚠️ **Never commit your `.env` file to GitHub.**

Add it to `.gitignore`:

```gitignore
.env
__pycache__/
*.pyc
.venv/
```

---

## 🚀 Getting Started

### Prerequisites

Make sure you have:

* Python 3.9+
* A Google Gemini API key
* Git
* Internet connection

---

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/gemini-cli-assistant.git
cd gemini-cli-assistant
```

### 2. Create a virtual environment

Windows:

```bash
python -m venv .venv
.venv\Scripts\activate
```

Linux / macOS:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install google-genai python-dotenv
```

Or, if `requirements.txt` is available:

```bash
pip install -r requirements.txt
```

### 4. Configure the API key

Create `.env`:

```env
GOOGLE_API_KEY=your_google_api_key_here
```

### 5. Run the application

```bash
python app.py
```

---

## 💻 Example

```text
Ask any question: Explain what an API is in simple terms

An API is a way for different software applications
to communicate with each other...
```

The application sends the question to Gemini and displays the generated response in the terminal.

---

## 🛠️ Technology Stack

| Technology           | Purpose                         |
| -------------------- | ------------------------------- |
| **Python**           | Application development         |
| **Google GenAI SDK** | Gemini API integration          |
| **Gemini 3.5 Flash** | Generative AI model             |
| **python-dotenv**    | Environment-variable management |
| **Git / GitHub**     | Version control                 |

---

## 🎯 Engineering Concepts Demonstrated

Although intentionally small, this project demonstrates several useful software-engineering concepts:

### Separation of Concerns

API configuration is separated from the application logic.

```text
app.py
   │
   └── Application / API logic

config.py
   │
   └── Environment configuration
```

### External API Integration

The application communicates with an external generative-AI service through the Google GenAI SDK.

### Secret Management

Credentials are retrieved through environment variables instead of being embedded directly into Python source code.

### Modular Design

The API-key retrieval logic is encapsulated inside a reusable function:

```python
def getthekey():
    ...
```

This makes configuration logic easier to replace or extend later.

---

# 📈 Future Improvements

The current implementation intentionally focuses on the basic AI request-response flow. The next engineering improvements could include:

### Phase 1 — Reliability

* [ ] Add API-key validation
* [ ] Handle missing `.env` configuration
* [ ] Add exception handling for API failures
* [ ] Handle network errors and timeouts
* [ ] Add graceful CLI exit

### Phase 2 — Better CLI Experience

* [ ] Add conversation history
* [ ] Support multi-turn conversations
* [ ] Add commands such as `/exit` and `/clear`
* [ ] Improve terminal formatting
* [ ] Add loading/progress indicators

### Phase 3 — Engineering Improvements

* [ ] Introduce structured logging
* [ ] Add unit tests
* [ ] Add type checking
* [ ] Add configuration validation
* [ ] Separate API service from CLI interface
* [ ] Add dependency locking

### Phase 4 — Production-Oriented Architecture

```text
CLI
 │
 ▼
Application Layer
 │
 ▼
AI Service Layer
 │
 ├── Gemini Client
 │
 ├── Error Handling
 │
 └── Retry Logic
 │
 ▼
Configuration Layer
 │
 └── Environment / Secret Management
```

This would allow the CLI to eventually be replaced by a REST API, web application, or other client without rewriting the core AI integration.

---

## 🧪 Testing Strategy

A production-oriented version of the project could introduce tests for:

```text
Configuration
    │
    ├── API key exists
    └── Missing API key handled correctly

AI Service
    │
    ├── Successful response
    ├── API failure
    └── Network failure

CLI
    │
    ├── User input
    ├── Empty input
    └── Exit command
```

This would make the application easier to maintain as additional functionality is introduced.

---

## 🔒 Security Considerations

This project follows a basic credential-separation approach, but a production implementation should go further.

### Never do this

```python
api_key = "AIza..."
```

### Prefer

```env
GOOGLE_API_KEY=...
```

and retrieve it through environment configuration.

Additional production considerations include:

* Secret managers instead of `.env`
* API-key rotation
* Request rate limiting
* Input validation
* Structured error handling
* Logging without exposing credentials
* Monitoring API usage

---

## 📊 Current Scope vs Production Scope

| Area           | Current Project      | Production Direction          |
| -------------- | -------------------- | ----------------------------- |
| Interface      | CLI                  | CLI / REST API / Web          |
| AI Model       | Gemini 3.5 Flash     | Configurable model layer      |
| Authentication | Environment variable | Secret manager                |
| Error Handling | Basic                | Structured exception handling |
| Testing        | Minimal              | Unit + integration tests      |
| Logging        | Basic                | Structured logging            |
| Conversation   | Single request       | Session-based history         |
| Deployment     | Local                | Cloud/container deployment    |

---

## 💡 Why This Project?

The goal of this project is not to hide complexity behind a framework.

Instead, it demonstrates the fundamental workflow behind an AI-powered application:

```text
INPUT
  ↓
CONFIGURATION
  ↓
API CLIENT
  ↓
MODEL REQUEST
  ↓
MODEL RESPONSE
  ↓
OUTPUT
```

Understanding this foundation makes it easier to build more sophisticated systems later.

---

## 🗺️ Roadmap

```text
                    CURRENT
                       │
                       ▼
              ┌─────────────────┐
              │ Gemini CLI App  │
              └────────┬────────┘
                       │
                       ▼
              Error Handling
                       │
                       ▼
             Conversation Memory
                       │
                       ▼
              Service Abstraction
                       │
                       ▼
                 REST API
                       │
                       ▼
              Database / Storage
                       │
                       ▼
             Containerization
                       │
                       ▼
               Cloud Deployment
```

---

## 🤝 Contributing

Contributions are welcome.

1. Fork the repository
2. Create a feature branch

```bash
git checkout -b feature/your-feature
```

3. Commit your changes

```bash
git commit -m "Add your feature"
```

4. Push the branch

```bash
git push origin feature/your-feature
```

5. Open a Pull Request

---

## 📜 License

This project is available for educational and development purposes.

---

## 👨‍💻 Author

**MD Zahid**

B.Tech — Computer Science & Engineering

Interested in:

`Python` • `Data Engineering` • `SQL` • `AI/ML` • `Backend Development`

---

<div align="center">

### ⭐ If you found this project useful, consider giving it a star!

**Built with Python + Google Gemini**

</div>
```
