# Python API LLM

A FastAPI-based REST API that leverages Ollama's Mistral model to generate AI-powered responses with built-in API key authentication and credit management.

## Overview

This project provides a simple yet powerful API endpoint for generating text using the Mistral large language model (LLM) through Ollama. It includes:

- **API Key Authentication**: Secure endpoint access using API key headers
- **Credit System**: Track and limit API usage with a credit-based system
- **FastAPI**: Modern, fast Python web framework for building APIs
- **Ollama Integration**: Direct integration with Ollama for LLM inference

## Prerequisites

- Python 3.7+
- [Ollama](https://ollama.ai/) installed and running locally
- Mistral model pulled in Ollama (`ollama pull mistral`)

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd Python-API-LLM
```

2. Create a virtual environment (optional but recommended):
```bash
python -m venv venv
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

4. Set up environment variables:
Create a `.env` file in the project root with your API key:
```
API_KEY=your-secret-api-key-here
```

## Running the Server

Start the Uvicorn server:
```bash
uvicorn main:app --reload
```

The API will be available at `http://127.0.0.1:8000`

Access the interactive API documentation:
- Swagger UI: `http://127.0.0.1:8000/docs`
- ReDoc: `http://127.0.0.1:8000/redoc`

## API Endpoints

### POST `/generate`

Generate text using the Mistral model.

**Request:**
```
POST /generate?prompt=Your+question+here
Headers:
  x-api-key: your-api-key
  Content-Type: application/json
```

**Parameters:**
- `prompt` (query string, required): The input text to send to the model

**Response:**
```json
{
  "response": "Generated text response from the model"
}
```

**Status Codes:**
- `200 OK`: Successful request
- `401 Unauthorized`: Invalid API key or insufficient credits

## Usage Example

```python
import requests
from dotenv import load_dotenv
import os

load_dotenv()

url = "http://127.0.0.1:8000/generate?prompt=Tell me about Python"
headers = {
    "x-api-key": os.getenv("API_KEY"),
    "Content-Type": "application/json"
}

response = requests.post(url, headers=headers)
print(response.json())
```

Or use the provided `test-api.py`:
```bash
python test-api.py
```

## Features

- **API Key Authentication**: All requests require a valid API key in the `x-api-key` header
- **Credit-Based Rate Limiting**: Each API key starts with 500 credits; each request costs 1 credit
- **Mistral Model**: Uses the powerful Mistral LLM for high-quality text generation
- **Error Handling**: Returns appropriate HTTP status codes and error messages

## Dependencies

- **fastapi**: Modern web framework for building APIs
- **uvicorn**: ASGI web server for running FastAPI
- **ollama**: Python client for Ollama LLM inference
- **python-dotenv**: Environment variable management
- **requests**: HTTP library for making requests

## Configuration

### API Keys and Credits

Modify the `API_KEY_CREDITS` dictionary in `main.py` to:
- Add new API keys with initial credit amounts
- Adjust credit limits as needed

```python
API_KEY_CREDITS = {
    "key1": 500,
    "key2": 1000,
}
```

### Model Selection

To change the LLM model, edit the model parameter in `main.py`:
```python
response = ollama.chat(model="your-model-name", messages=[...])
```

## Troubleshooting

**"Connection refused" error:**
- Ensure Ollama is running: `ollama serve`
- Verify the model is pulled: `ollama pull mistral`

**"Invalid API Key" error:**
- Check your `.env` file contains the correct API key
- Verify the key matches what's configured in `main.py`
- Ensure you have credits remaining

**"Model not found" error:**
- Pull the Mistral model: `ollama pull mistral`

## Future Enhancements

- Database integration for persistent credit management
- Support for multiple LLM models
- Request logging and analytics
- Rate limiting per API key
- Batch processing endpoints
- Streaming responses

## License

[Add your license here]

## Support

For issues or questions, please open an issue on the repository.

