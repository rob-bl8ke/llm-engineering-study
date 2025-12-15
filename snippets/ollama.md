

# Ollama - Verify and pull local models
- Fetch local model information using http.
- List available models.
- Check that the required models for this piece of code exist locally.
- Pull the latest models if they're missing.


```python
# Check if Ollama is running and verify models
try:
    import requests
    response = requests.get("http://localhost:11434/")
    print("✅ Ollama is running!")
    
    # Check available models
    models_response = requests.get("http://localhost:11434/api/tags")
    if models_response.status_code == 200:
        models = models_response.json()
        available_models = [model['name'] for model in models.get('models', [])]
        print(f"📋 Available models: {available_models}")
        
        # Check for our required models
        required_models = ["llama3.2:latest"]
        missing_models = [model for model in required_models if model not in available_models]
        
        # Pull the latest models if missing.
        if missing_models:
            print(f"⚠️  Missing models: {missing_models}")
            print("Please pull them with:")
            for model in missing_models:
                print(f"  ollama pull {model}")
        else:
            print("✅ All required models are available!")
            
except Exception as e:
    print(f"❌ Ollama connection error: {e}")
    print("Please start Ollama with: ollama serve")
```