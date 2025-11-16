# Ollama Setup Guide for Puter.js App

## 🔧 Prerequisites

1. **Ollama Installed**: Download from https://ollama.ai
2. **At least one model downloaded**: `ollama pull llama2`

## 🌐 Enable CORS on Ollama

By default, Ollama blocks browser requests due to CORS (Cross-Origin Resource Sharing) restrictions. You need to enable CORS to allow the Puter.js app to communicate with your local Ollama instance.

### Method 1: Environment Variable (Recommended)

#### On Linux/Mac:
```bash
OLLAMA_ORIGINS=* ollama serve
```

#### On Windows (PowerShell):
```powershell
$env:OLLAMA_ORIGINS="*"
ollama serve
```

#### On Windows (Command Prompt):
```cmd
set OLLAMA_ORIGINS=*
ollama serve
```

### Method 2: Systemd Service (Linux - Persistent)

1. Edit the Ollama service file:
```bash
sudo systemctl edit ollama.service
```

2. Add this configuration:
```ini
[Service]
Environment="OLLAMA_ORIGINS=*"
```

3. Reload and restart:
```bash
sudo systemctl daemon-reload
sudo systemctl restart ollama
```

### Method 3: Launchd (macOS - Persistent)

1. Create/edit the environment file:
```bash
launchctl setenv OLLAMA_ORIGINS "*"
```

2. Restart Ollama:
```bash
brew services restart ollama
```

## 📋 Step-by-Step Usage

### 1. Start Ollama with CORS
```bash
OLLAMA_ORIGINS=* ollama serve
```

### 2. Verify Ollama is Running
Open a new terminal and run:
```bash
curl http://localhost:11434/api/tags
```

You should see a JSON response with your available models.

### 3. Pull a Model (if you haven't already)
```bash
ollama pull llama2
# or
ollama pull mistral
# or
ollama pull codellama
```

### 4. Open the Puter.js App
Open `index.html` in your browser.

### 5. Test the Connection
1. In the sidebar under "Local Ollama Models"
2. Make sure the URL is `http://localhost:11434`
3. Click "🔌 Test Connection"
4. You should see a success message

### 6. Load Available Models
Click "📋 Load Available Models" to automatically fetch all models from your Ollama instance.

### 7. Start Chatting
1. Select one of your Ollama models from the list
2. Type your message and send
3. The app will communicate directly with your local Ollama instance

## 🔍 Troubleshooting

### Issue: "Failed to connect to Ollama"

**Solution 1**: Make sure Ollama is running with CORS enabled
```bash
OLLAMA_ORIGINS=* ollama serve
```

**Solution 2**: Check if Ollama is actually running
```bash
ps aux | grep ollama
```

**Solution 3**: Verify the port (default is 11434)
```bash
curl http://localhost:11434/api/tags
```

### Issue: "CORS policy: No 'Access-Control-Allow-Origin' header"

This means Ollama is running but CORS is not enabled. Restart Ollama with:
```bash
OLLAMA_ORIGINS=* ollama serve
```

### Issue: "Connection refused"

**Possible causes**:
1. Ollama is not running → Start it with `ollama serve`
2. Wrong port → Check if Ollama is using a different port
3. Firewall blocking → Allow port 11434 in your firewall

### Issue: "No models found"

Pull a model first:
```bash
ollama pull llama2
```

Then click "Load Available Models" again.

## 🔐 Security Note

Using `OLLAMA_ORIGINS=*` allows all origins to access your Ollama instance. This is fine for local development, but if you want to restrict access to only your Puter.js app, you can specify the exact origin:

```bash
OLLAMA_ORIGINS=http://localhost:8080 ollama serve
```

Or if hosting the app:
```bash
OLLAMA_ORIGINS=https://yourdomain.com ollama serve
```

For multiple origins:
```bash
OLLAMA_ORIGINS="http://localhost:8080,https://yourdomain.com" ollama serve
```

## 📦 Popular Ollama Models to Try

| Model | Command | Best For |
|-------|---------|----------|
| Llama 2 | `ollama pull llama2` | General conversation |
| Llama 2 7B | `ollama pull llama2:7b` | Fast responses |
| Llama 2 13B | `ollama pull llama2:13b` | Better quality |
| Mistral | `ollama pull mistral` | Excellent reasoning |
| CodeLlama | `ollama pull codellama` | Code generation |
| Phi-2 | `ollama pull phi` | Lightweight, fast |
| Neural Chat | `ollama pull neural-chat` | Conversational |
| Starling | `ollama pull starling-lm` | High quality |
| Orca Mini | `ollama pull orca-mini` | Small but capable |
| Vicuna | `ollama pull vicuna` | Instruction following |

## 🚀 Advanced Configuration

### Change Ollama Port

```bash
OLLAMA_HOST=0.0.0.0:8080 OLLAMA_ORIGINS=* ollama serve
```

Then update the URL in the app to `http://localhost:8080`

### Use Custom Models

```bash
# Create a Modelfile
cat > Modelfile <<EOF
FROM llama2
SYSTEM You are a helpful assistant specialized in JavaScript.
EOF

# Create custom model
ollama create my-js-assistant -f Modelfile

# Use it in the app
```

### Enable GPU Acceleration

Ollama automatically uses GPU if available. To verify:
```bash
ollama run llama2 --verbose
```

## 💡 Tips

1. **Faster Responses**: Use smaller models like `phi`, `orca-mini`, or `llama2:7b`
2. **Better Quality**: Use larger models like `llama2:13b` or `mistral`
3. **Multiple Models**: You can have multiple models installed and switch between them
4. **Model Management**: Use `ollama list` to see all installed models
5. **Remove Models**: Use `ollama rm model-name` to free up space

## 🆘 Getting Help

- Ollama Documentation: https://github.com/ollama/ollama/blob/main/docs/
- Ollama Discord: https://discord.gg/ollama
- Check Ollama logs: `journalctl -u ollama -f` (Linux)

## 📝 Quick Reference Commands

```bash
# Start Ollama with CORS
OLLAMA_ORIGINS=* ollama serve

# List installed models
ollama list

# Pull a new model
ollama pull model-name

# Remove a model
ollama rm model-name

# Test Ollama is running
curl http://localhost:11434/api/tags

# Run a model in terminal
ollama run llama2

# Get model info
ollama show llama2
```

---

**You're all set! Enjoy using local AI models with your Puter.js app! 🎉**
