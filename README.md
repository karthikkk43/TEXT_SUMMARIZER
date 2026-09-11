# Text Summarizer

I fine-tuned T5-small on the SAMSum dataset to summarize chat conversations,
then wrapped it in a FastAPI app so you can paste a dialogue and get a summary
back.

## Files

- `textsumbase.py` — trains the model
- `app.py` — the API and server
- `index.html` — the web page

The trained model isn't in the repo since it's a few hundred MB. You'll need to
run the training script first to create it.

## How it works

SAMSum is a bunch of messenger conversations with summaries written by humans.
I took 4,000 of them, cleaned up the text, and fine-tuned t5-small for 6 epochs.
Took about 70 minutes on my M-series Mac.

The app loads that trained model and runs it whenever you send it a dialogue.

## Running it

Install what you need:

```bash
pip install torch transformers sentencepiece pandas fastapi uvicorn jinja2
```

Train the model (you need the SAMSum CSVs in the folder):

```bash
python textsumbase.py
```

Start the server:

```bash
uvicorn app:app --reload
```

Then go to http://127.0.0.1:8000

## Things worth knowing

Summaries come out all lowercase — that's because I lowercase the text during
cleaning. Removing `.lower()` and retraining would fix it.

It's trained on casual chat, so it does well on conversations and worse on
formal or technical writing.

Dataset: https://huggingface.co/datasets/knkarthick/samsum
