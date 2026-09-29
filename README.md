<div align="center">

# Agentic Legal Assistant — Multi-Tool AI Agent

**An autonomous legal research agent** built on the Google Gen AI SDK with
Gemini 2.5 Flash. It runs a system-driven persona, decides which tools to call,
executes them, and iterates on its answer until it has enough grounding.

`Gemini 2.5 Flash` `Google Gen AI SDK` `Jupyter` `Tool Calling`

</div>

---

## What this demonstrates

The point of this project is the **agent loop**, not the legal domain. It shows
the three things that turn a single prompt into an agent:

1. **System-driven persona** — a persistent instruction block that shapes how
   the model reasons and what it refuses to do
2. **Automated function calling** — the model emits a tool call, your code
   executes the real function, and the result is fed back in
3. **Iterative response loops** — the agent re-reads its own output and
   decides whether to call another tool or answer

The same skeleton supports any tool-equipped agent. Swapping the legal tools for
a different domain is the intended reuse.

---

## Setup

This is a **Colab notebook**. Open it in Google Colab and run the cells in
order.

You'll need a [Gemini API key](https://aistudio.google.com/apikey). The notebook
prompts for it with `input()` and sets `GOOGLE_API_KEY` in the environment.

> The API key used to be echoed into this notebook's saved cell output and was
> committed to the repository. It has been removed from the output, but it is
> still in git history. If it was ever a real key, revoke it at
> [aistudio.google.com/apikey](https://aistudio.google.com/apikey).

For a cleaner setup, store the key as a Colab Secret instead of typing it:

```python
from google.colab import userdata
import os
os.environ['GOOGLE_API_KEY'] = userdata.get('GOOGLE_API_KEY')
```

---

## Security note

The `input()` approach means the key appears in a cell's stdout, and Colab
persists that output into the `.ipynb` when you save. **Use Colab Secrets, or
clear outputs before committing a notebook.** This is the most common way
API keys leak from notebooks.

---

## Limitations

- **Not legal advice.** The persona is a prompt convention, not a
  qualification. Do not use this output for real legal decisions.
- **Tool results are unverified.** The agent trusts whatever a tool returns;
  there is no citation checking or grounding audit.
- Single-file, notebook-scoped. No error handling beyond try/except, no tests,
  no persistence between sessions.

---

## Repository layout

| Path | Contents |
| --- | --- |
| `Agentic_legal&ResearchAssistant.ipynb` | The entire agent — persona, tools, loop |
| `documents/` | Engineering specs: requirements, data-flow diagram, use cases, architecture |

The `&` in the filename is a Colab export artifact.

## What changed (v3)

- v1→v2: research-based market analysis + SDLC documentation (`documents/01–07`).
- v2→v3: delivery roadmap with sprint plan and ceremonies
  (`documents/08-roadmap.md`); this changelog. No source code changed.

## License

MIT — use commercially, no attribution required.
