
# Intelligent PDF Summarizer

This project is an Azure Durable Functions-based application that summarizes PDF documents using OpenAI's GPT language model. The system extracts text content from uploaded PDFs and returns concise summaries through an orchestrated function flow.

## Features

- Upload PDF files via HTTP trigger
- Extract text content using Azure Form Recognizer or local parsing
- Generate intelligent summaries using OpenAI (gpt-3.5-turbo)
- Designed with Azure Durable Functions orchestration
- Modular activity functions for scalability

## Why Not Using Azure OpenAI?

This project originally aimed to use Azure OpenAI Service. However, due to subscription and regional restrictions, it was not accessible. Therefore, the solution uses the official OpenAI API (https://platform.openai.com) with a personal API key. Functionality remains the same.

## Technologies Used

- Azure Functions (Python)
- Azure Durable Functions
- OpenAI GPT API
- Azure Blob Storage
- Azure Form Recognizer
- Python 3.11+
- PyMuPDF / fitz (PDF parsing)

## Project Structure

```
Intelligent-PDF-Summarizer/
├── HttpTrigger/__init__.py              # Entry point for PDF upload trigger
├── HttpTrigger/function.json
├── OrchestratorFunction/__init__.py     # Coordinates the workflow
├── OrchestratorFunction/function.json
├── ActivityFunctions/                   # Sub-tasks such as extract_text and summarize_text
│   ├── extract_text.py
│   └── summarize_text.py
├── shared_code/                         # Helper functions and shared utils
│   └── openai_client.py
├── local.settings.json.example          # Configuration template (without secrets)
├── requirements.txt                     # Python dependencies
├── .gitignore
├── azure.yaml                           # Azure deployment settings
├── host.json
```

## Configuration (local.settings.json)

Do **not** commit secrets. Instead, use the following template and set your keys locally:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "BLOB_STORAGE_CONNECTION_STRING": "YOUR_CONNECTION_STRING",
    "COGNITIVE_SERVICES_ENDPOINT": "https://your-resource.cognitiveservices.azure.com/",
    "COGNITIVE_SERVICES_KEY": "YOUR_KEY",
    "OPENAI_API_KEY": "YOUR_OPENAI_API_KEY",
    "CHAT_MODEL_NAME": "gpt-3.5-turbo"
  }
}
```

## How to Run Locally

1. Clone the repository:

```bash
git clone https://github.com/Ethan05302/Intelligent-PDF-Summarizer.git
cd Intelligent-PDF-Summarizer
```

2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Create `local.settings.json` from the example and add real API keys.

4. Start the Azure Function host:

```bash
func start
```

5. Use Postman or REST Client to send a POST request with a PDF file to the HTTP trigger.
