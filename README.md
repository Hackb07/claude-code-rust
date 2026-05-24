[![progress-banner](https://backend.codecrafters.io/progress/claude-code/ae83f53a-73d2-48a1-9b6b-be1d83238f82)](https://app.codecrafters.io/users/codecrafters-bot?r=2qF)

# CodeCrafters Claude Code — Rust

A minimal AI coding assistant built in Rust as part of the
["Build Your own Claude Code" Challenge](https://codecrafters.io/challenges/claude-code).

Uses LLMs via OpenRouter (OpenAI-compatible API) with an agent loop that
advertises tools, executes them, and feeds results back to the model until the
task is complete.

## Features

- **Agent Loop** — Continuously sends conversation history + tools to the LLM,
  processes tool calls, and repeats until a final text response is produced.
- **Read Tool** — Reads a file from the filesystem and returns its contents.
- **Write Tool** — Writes content to a file (creates or overwrites).
- **Bash Tool** — Executes shell commands and captures stdout/stderr output.

## Architecture

```mermaid
flowchart TD
    A["User Prompt"] --> B["Initialize Messages<br>[role: user]"]
    B --> C["Send to LLM API<br>(OpenRouter)"]
    C --> D{"Response has<br>tool_calls?"}
    D -- Yes --> E["Execute Tool<br>(Read / Write / Bash)"]
    E --> F["Append Tool Result<br>[role: tool]"]
    F --> C
    D -- No --> G["Print Final Response"]
    G --> H["Exit"]
```

## Usage

```sh
export OPENROUTER_API_KEY="sk-..."
./your_program.sh -p "What is the content of apple.py?"
```

## Tools

```mermaid
flowchart LR
    subgraph LLM["LLM"]
        TC["tool_calls"]
    end
    subgraph Agent["Agent"]
        R["Read"]
        W["Write"]
        B["Bash"]
    end
    TC -- "Read<br>file_path" --> R
    TC -- "Write<br>file_path, content" --> W
    TC -- "Bash<br>command" --> B
    R --> Result["File Contents"]
    W --> Result["Confirmation"]
    B --> Result["stdout + stderr"]
    Result --> Agent
```

## Build

Requires Rust 1.95+.

```sh
cargo build --release
```
