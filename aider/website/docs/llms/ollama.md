---
parent: Connecting to LLMs
nav_order: 500
---

# Ollama

Aider can connect to local Ollama models.

First, install aider:

{% include install.md %}

Then configure your Ollama API endpoint (usually the default):

```bash
export OLLAMA_API_BASE=http://127.0.0.1:11434 # Mac/Linux
setx   OLLAMA_API_BASE http://127.0.0.1:11434 # Windows, restart shell after setx
```

Start working with aider and Ollama on your codebase:

```
# Pull the model
ollama pull <model>

# Start your ollama server, increasing the context window to 8k tokens
OLLAMA_CONTEXT_LENGTH=8192 ollama serve

# In another terminal window, change directory into your codebase
cd /to/your/project

aider --model ollama_chat/<model>
```

### Local code apply model

You can run a local model to handle applying edits. Start ollama and specify a tag:

```
ollama pull osmosis/osmosis-apply-1.7B
OLLAMA_CONTEXT_LENGTH=8192 ollama serve

aider --apply-model-tag <tag>
```

This uses `ollama_chat/osmosis/osmosis-apply-1.7B:<tag>` as the editor model.

You can customize the instructions sent to this model with `--apply-prompt "YOUR PROMPT"`.

{: .note }
Using `ollama_chat/` is recommended over `ollama/`.


See the [model warnings](warnings.html)
section for information on warnings which will occur
when working with models that aider is not familiar with.

## API Key

If you are using an ollama that requires an API key you can set `OLLAMA_API_KEY`:

```
export OLLAMA_API_KEY=<api-key> # Mac/Linux
setx   OLLAMA_API_KEY <api-key> # Windows, restart shell after setx
```

## Setting the context window size

[Ollama uses a 2k context window by default](https://github.com/ollama/ollama/blob/main/docs/faq.md#how-can-i-specify-the-context-window-size),
which is very small for working with aider.
It also **silently** discards context that exceeds the window. 
This is especially dangerous because many users don't even realize that most of their data
is being discarded by Ollama.
 
By default, aider sets Ollama's context window 
to be large enough for each request you send plus 8k tokens for the reply.
This ensures data isn't silently discarded by Ollama.

If you'd like you can configure a fixed sized context window instead
with an
[`.aider.model.settings.yml` file](https://aider.chat/docs/config/adv-model-settings.html#model-settings)
like this:

```
- name: ollama/qwen2.5-coder:32b-instruct-fp16
  extra_params:
    num_ctx: 65536
```

