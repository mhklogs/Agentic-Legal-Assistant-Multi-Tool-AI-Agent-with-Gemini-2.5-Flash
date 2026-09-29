# Notes

## Run

Open `Agentic_legal&ResearchAssistant.ipynb` in Colab and run all cells top to bottom.
Cell order matters: cell 1 installs/imports, cell 2 authenticates, cells 3–4 define the
agent, cell 5 is the exercise sheet.

## API key

The notebook never stores a key. Cell 2 prompts for one and exports it to
`os.environ['GOOGLE_API_KEY']`; `genai.Client()` picks it up from there. For a saved
secret, uncomment the `userdata.get('GEMINI_API_KEY')` line in that cell.

An earlier run of this notebook echoed the typed key into the cell output. That output is
now redacted at HEAD, but the key may still exist in git history — rotate it if this repo
is public.

## Dependencies

```
pip install google-genai
```

Colab provides the `google.colab` module; outside Colab, replace the cell 2 key prompt with
an `os.environ` read.
