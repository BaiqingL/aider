---
parent: More info
nav_order: 700
description: Using a local model to apply patches
---

# Local code apply model

Aider can delegate the task of applying patches to a local model hosted by Ollama.

Run the following commands to pull and start the model:

```bash
ollama pull osmosis/osmosis-apply-1.7B
OLLAMA_CONTEXT_LENGTH=8192 ollama serve
```

When you start aider, specify the tag for the apply model:

```bash
aider --apply-model-tag <tag>
```

This will use `ollama_chat/osmosis/osmosis-apply-1.7B:<tag>` to apply code edits.
You can override the system prompt sent to this model with `--apply-prompt "YOUR PROMPT"`.
