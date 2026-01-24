# LLM Engineering Study

`uv` - An extremely fast Python package manager.

## Setup Options

### Option 1: Dev Container (Recommended)

The fastest way to get started is using the Dev Container, which provides a fully configured environment:

1. **Prerequisites:**
   - [Docker Desktop](https://www.docker.com/products/docker-desktop) installed and running
   - [VS Code](https://code.visualstudio.com/) with the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

2. **Launch the Dev Container:**
   - Open this folder in VS Code
   - Press `F1` and select **Dev Containers: Reopen in Container**
   - VS Code will build the container and install all dependencies automatically

3. **Configure API Keys:**
   - Create a `.env` file in the workspace root
   - Add your API keys (OpenAI, Anthropic, Google, etc.)

The Dev Container includes:
- Python 3.11 with `uv` for fast package management
- All project dependencies pre-installed via `uv sync`
- Jupyter, Python, and Pylance extensions
- GitHub Copilot (if you have access)
- Git tools (GitLens, Git Graph)
- Gradio port (7860) automatically forwarded

### Option 2: Local Setup

If you prefer to work locally without Docker:

#### Step 1: Clone and Open
- Clone this repo and open it in VS Code or Cursor

#### Step 2: Install `uv`
Follow the [official installation guide](https://docs.astral.sh/uv/getting-started/installation/)

```bash
# Verify uv installation
uv --version
uv self update
```

#### Step 3: Set up the environment
```bash
# Create virtual environment and install dependencies
uv sync
```

#### Step 4: Configure VS Code
- Install the Python and Jupyter extensions from Microsoft
- Select the `.venv` Python environment in Jupyter notebooks (top right of `.ipynb` files)

#### Step 5: Add API Keys
- Create a `.env` file in the project root
- Add your OpenAI key and any other required API keys

## Additional Resources

- [Original setup instructions](https://github.com/ed-donner/llm_engineering/blob/main/setup/SETUP-new.md)
- [Udemy Course](https://www.udemy.com/course/llm-engineering-master-ai-and-large-language-models/)

## Dev Container Extensions

The following VS Code extensions are automatically installed in the Dev Container:
- Python (`ms-python.python`)
- Pylance (`ms-python.vscode-pylance`)
- Jupyter (`ms-toolsai.jupyter`)
- GitHub Copilot (`github.copilot`, `github.copilot-chat`)
- GitLens (`eamodio.gitlens`)
- Git Graph (`mhutchie.git-graph`)
- DotENV (`mikestead.dotenv`)
- Todo Tree (`gruntfuggly.todo-tree`)
- Material Icon Theme (`pkief.material-icon-theme`)
