# Local AI Development Environment

My personal setup for running top-tier 32B+ open-weight LLMs locally for data engineering, Python, and SQL development.

## Hardware Setup
- **CPU:** AMD Ryzen 9 9950X3D
- **GPU:** Zotac GeForce RTX 5090 Solid (32GB VRAM)
- **RAM:** 96GB DDR5 6000MHz
- **Storage:** Gen5 NVMe
- **OS:** Windows 11 with Ubuntu WSL2

## The Models
This setup utilizes the 32GB of VRAM on the RTX 5090 to run 4-bit quantized ~30B parameter models via Ollama.
- **Qwen 3.6 (35B-A3B):** General coding, DuckDB/PySpark scripting, and fast iteration. Uses Mixture of Experts (MoE) for speed.
- **DeepSeek R1 (32B):** Chain-of-thought model for complex architecture, logic puzzles, and deep debugging.

## Installation & Setup

### 1. Install Dependencies (WSL2)
Require `zstd` to unpack Ollama binaries:
sudo apt-get update && sudo apt-get install zstd -y

### 2. Install Ollama
Install the native Linux binary to utilize WSL2 GPU passthrough:
curl -fsSL https://ollama.com/install.sh | sh

### 3. Pull the Models
ollama run qwen3.6:35b-a3b
ollama run deepseek-r1:32b

### 4. Setup Open WebUI (Local ChatGPT Interface)
Run the web interface via Docker using host networking to access native Ollama on WSL2:
docker run -d --network="host" -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main

Access the UI at: http://localhost:3000

### 5. IDE Integration (VS Code via Continue.dev)

1. **Install the Extension:** 
   - Open VS Code.
   - Go to the Extensions view (Ctrl+Shift+X).
   - Search for **Continue** and install the official extension.

2. **Open the Config:**
   - Once installed, click the Continue icon in your VS Code sidebar.
   - At the bottom of the Continue sidebar, click the **Gear (Config) icon**. This opens your `config.json` file.

3. **Update the Model List:**
   - Locate the "models" array inside that JSON file.
   - Add your local Ollama endpoints by pasting this block inside the "models" array:

   {
     "title": "Qwen 3.6 Coder",
     "provider": "ollama",
     "model": "qwen3.6:35b-a3b"
   },
   {
     "title": "DeepSeek R1",
     "provider": "ollama",
     "model": "deepseek-r1:32b"
   }

4. **Activate it:**
   - Save the `config.json` file.
   - Go back to the Continue sidebar. You will now see your models in the dropdown at the bottom. Select one of them to make it active.

5. **Start using it:**
   - **Chat:** Click the Continue sidebar to chat normally (Ctrl+L).
   - **Inline Edit:** Highlight any block of code, press Ctrl+I, and tell the model what to change (e.g., "Refactor this to use DuckDB instead of pandas").
   - **Context:** Continue automatically indexes open files, so you can ask it to explain how different scripts interact by using context tags like @file or @codebase.
