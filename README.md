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
   - Press `Ctrl + Shift + P` (or `F1`) to open the VS Code Command Palette.
   - Type `Continue: Open Config File` and hit Enter to open `config.yaml`.
   - *(Note: Ensure you are editing the Windows host file at `%USERPROFILE%\.continue\config.yaml` rather than a blank Linux one).*

3. **Update the Model List:**
   - Replace the default contents of `config.yaml` with your custom local models:

   ```yaml
   name: Main Config
   version: 1.0.0
   schema: v1
   models:
     - name: Qwen 3.6 Coder
       provider: ollama
       model: qwen3.6:35b-a3b
     - name: DeepSeek R1
       provider: ollama
       model: deepseek-r1:32b
