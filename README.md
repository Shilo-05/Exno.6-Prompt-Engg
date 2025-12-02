# EX-06 Development of Python Code Compatible with Multiple AI Tools

```text
Name: Oswald Shilo  
Reg.No: 212223040139  
```

## AIM

To develop a modular Python script that integrates with multiple AI tools (e.g., OpenAI, Cohere) via their APIs, enabling automated testing and comparison of model responses to specific prompts.


## SCENARIO

**Context:**  
You are a Backend Engineer at a startup building a content generation platform. The product manager wants to know which LLM (Large Language Model) provides the most concise and accurate answers for general knowledge queries before purchasing a commercial license.

**Task:**  
Write a Python script that sends the same prompt  
> "What is the capital of France?"  
to two different AI providers (OpenAI and Cohere), retrieves the responses, and prints them for side-by-side comparison.

## ALGORITHM

1. **Library Import**  
   Import necessary Python libraries (`requests`, `json`, `openai`) to handle HTTP requests and JSON data.

2. **API Key Configuration**  
   Set up authentication variables for the targeted AI platforms.

3. **Define Interface Functions**  
   - `get_chatgpt_response()` to handle the OpenAI API endpoint structure.  
   - `get_cohere_response()` to handle the Cohere API endpoint structure.

4. **Main Execution Flow**  
   - Define a standardized test prompt.  
   - Call both functions with the prompt.  
   - Capture and store the outputs.

5. **Output & Comparison**  
   Print the results to the console for analysis and comparison.

<img width="490" height="489" alt="image" src="https://github.com/user-attachments/assets/31fb31ff-c1eb-4fa9-9227-6265fddb7b85" />


## PYTHON CODE IMPLEMENTATION

```python
import openai
import requests
import json

# Configuration (Replace with actual keys for execution)
OPENAI_API_KEY = "sk-xxxx-your-openai-key"
COHERE_API_KEY = "xxxx-your-cohere-key"

# Function to interact with ChatGPT (OpenAI API)
def get_chatgpt_response(prompt: str) -> str:
    try:
        headers = {
            "Content-Type": "application/json",
            "Authorization": f"Bearer {OPENAI_API_KEY}"
        }
        data = {
            "model": "gpt-3.5-turbo-instruct",
            "prompt": prompt,
            "max_tokens": 50
        }
        response = requests.post(
            "https://api.openai.com/v1/completions",
            headers=headers,
            json=data
        )
        response.raise_for_status()
        return response.json()["choices"][0]["text"].strip()
    except Exception as e:
        return f"Error connecting to OpenAI: {e}"

# Function to interact with Cohere API
def get_cohere_response(prompt: str) -> str:
    try:
        api_url = "https://api.cohere.ai/v1/generate"
        headers = {
            "Authorization": f"Bearer {COHERE_API_KEY}",
            "Content-Type": "application/json"
        }
        data = {
            "model": "command",
            "prompt": prompt,
            "max_tokens": 50
        }
        response = requests.post(api_url, headers=headers, json=data)
        response.raise_for_status()
        return response.json()["generations"][0]["text"].strip()
    except Exception as e:
        return f"Error connecting to Cohere: {e}"

# --- Main Execution ---

if __name__ == "__main__":
    # Define the test prompt
    test_prompt = "What is the capital of France? Answer in one sentence."

    print(f"--- Benchmarking Prompt: '{test_prompt}' ---\n")

    # 1. Get ChatGPT Output
    print("Requesting ChatGPT...")
    chatgpt_output = get_chatgpt_response(test_prompt)

    # 2. Get Cohere Output
    print("Requesting Cohere...")
    cohere_output = get_cohere_response(test_prompt)

    # 3. Compare Results
    print("\n--- Comparative Results ---")
    print(f"ChatGPT Response: {chatgpt_output}")
    print(f"Cohere Response:  {cohere_output}")
```

## SIMULATED OUTPUT

Since the code requires valid API keys and internet access, below is a simulated example of a successful run:
**Benchmarking Prompt:** 'What is the capital of France? Answer in one sentence.' ---

Requesting ChatGPT...
Requesting Cohere...

**Comparative Results**
ChatGPT Response: The capital of France is Paris.
Cohere Response:  Paris is the capital city of France.
COMPARATIVE ANALYSIS
Feature	OpenAI (ChatGPT)	Cohere	Insights
Accuracy	High	High	Both models correctly identified Paris as the capital.
Conciseness	Direct, strictly one simple sentence	One sentence, slightly more descriptive	Both follow instructions; OpenAI is slightly more minimal.
Style	Straightforward, factual	Slightly more narrative tone	Choice depends on product tone requirements.
Integration	Uses /v1/completions endpoint	Uses /v1/generate endpoint	Script cleanly abstracts both via separate functions.


## RESULT
The Python script was successfully designed to work with multiple AI tools by:

Using modular functions for each provider (OpenAI and Cohere).

Sending the same standardized prompt to both APIs.

Printing the responses for quick, side-by-side comparison.

This modular approach enables easy extension to additional providers in the future and supports systematic benchmarking of LLM performance for the startup’s content generation platform.
