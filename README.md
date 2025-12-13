
`uv` - An extremely fast Python package manager.

# Creating a full powered Data Science Environment

- Here is a [direct link to the setup instructions](https://github.com/ed-donner/llm_engineering/blob/main/setup/SETUP-new.md)
    
    [Step 1](https://www.udemy.com/course/llm-engineering-master-ai-and-large-language-models/learn/lecture/52932175#overview) - [Clone the repo](https://github.com/ed-donner/llm_engineering), [install Cursor](https://cursor.com/) (but you’ll be fine with VS Code) and open the project.
    
    Step 2 - [Install](https://github.com/ed-donner/llm_engineering/blob/main/setup/SETUP-new.md#step-2-installing-the-fabulous-uv-then-doing-a-uv-sync) the wonderful `uv` and set up your environment. Here’s a direct link: https://docs.astral.sh/uv/getting-started/installation/
    
    Step 3 - Create an OpenAI key.
    
    Step 4 - Create the .env file in the root of your project `llm_engineering` add your OpenAI key.
    
    Step 5 - Open Visual Studio Code - Install Jupyter and Python extensions from Microsoft.
    

```bash
# Once UV is installed, run these commands to verify and ensure you have
# the latest version
uv --version
uv self update

# This command creates a virtual environment (.venv) folder.
uv sync
```

Now, at the top right of your `.ipynb` file you can select your environment. Choose Python Environment and then choose  the `.venv` one.