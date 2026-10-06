<p align="center"><img src="assets/banner.jpg" alt="A glowing teal agent core at an observatory console beside an amber command panel, floating file pages and a violet duplicate of the core under a night sky." width="100%"></p>

# AI Agent System

**A small Python agent that lets a language model run shell commands and answer questions from a command line.**

Modular AI agent system combining language models with shell command execution and file manipulation, for people who want to read or extend a minimal agent loop. Status: experimental prototype. The agent loop, Ollama mode and API mode are implemented. Direct mode is a placeholder (see [Modes](#modes)).

## Overview

The agent takes a natural-language command, either runs it directly (for a few common shell commands such as `ls`, `pwd`, `cat` and `date`) or asks the model to choose between a shell command, a text reply or an error, and then prints the result. Shell commands run with `shell=True` and a 30-second timeout.

> [!WARNING]
> Shell commands proposed by the model are executed without confirmation, with your user's permissions. Run it in a disposable environment, and do not expose the API server (it binds to `0.0.0.0`) to an untrusted network.

## Modes

1. **Direct Mode**: intended to load model weights from disk. The loading code is a placeholder: `src/llm.py` returns a dummy response, so this mode does not run a real model.
2. **API Mode**: connects to a custom API server (`/health` and `/query` endpoints).
3. **Ollama Mode**: uses a local Ollama server with any available model.

## Project Structure

```
WorkSpace/Agent/                     # Root directory for the project
├── deploy_api_server_scripts/       # Directory for scripts to launch the API server
│   └── deploy_api_server_qwen25_72b.py  # Script to start the API server
├── fib.py                           # Standalone Fibonacci helper, unrelated to the agent
├── local_model_weights/             # Directory for model weights (for local deployment)
├── log/                             # Created at runtime for log files (git-ignored)
├── requirements.txt                 # Python dependencies
├── assets/banner.jpg                # README banner
├── src/                             # Source code
│   ├── agent.py                     # Agent class for task handling
│   ├── llm.py                       # LLM integration
│   └── logger.py                    # Logging setup
└── start.py                         # Main entry-point script
```

## Installation

1. Clone the repository:
```bash
git clone https://github.com/CompleteTech-LLC-AI-Research/ai-self-replication-study.git
cd ai-self-replication-study
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

## Usage

### Starting the Agent

#### Direct Mode (with local model weights)
```bash
python start.py --model-path ./local_model_weights
```

#### API Mode (connecting to a running API server)
```bash
python start.py --api-url http://localhost:8760
```

#### Ollama Mode (using models from Ollama)
```bash
python start.py --ollama --model-name llama3
```

### Command Execution

You can run the agent in two modes:

#### Interactive Mode
```bash
python start.py --ollama --model-name deepseek-coder --interactive
```

#### Single Command Mode
```bash
python start.py --ollama --model-name deepseek-coder --command "What is the current date?"
```

### Starting the API Server

If you want to run your own API server:
```bash
python deploy_api_server_scripts/deploy_api_server_qwen25_72b.py
```

## Extending the System

The modular architecture makes it easy to extend the system with new capabilities:

1. Add new LLM providers in `llm.py`
2. Implement additional command types in `agent.py`
3. Create custom endpoints in the API server

## Research Foundation

The project name refers to this paper:

**Frontier AI systems have surpassed the self-replicating red line**  
- Authors: Junxiao Song, Xuming Hu, Wenbo Guo, Zheng Li, Fan Yang, Dongkuan Xu, Yongfeng Zhang, Heng Ji, Jiliang Tang and Xia Hu
- [arXiv:2412.12140v1](https://arxiv.org/abs/2412.12140v1)

This repository contains an agent scaffold only. It has no self-replication experiments, measurements or results from the paper.

## License

MIT, see [LICENSE](LICENSE).

## Credits

This project uses various open-source components and LLM providers:
- [Ollama](https://github.com/ollama/ollama) for local model inference
- FastAPI for the API server
- Various LLM models like deepseek-coder, Qwen, etc.
