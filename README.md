# Agentic RAG - Interview Q&A

I wanted to actually build something with RAG instead of just reading interview questions about it, so I took an open-source agentic RAG project and pointed it at my own interview prep PDFs to see how it holds up on real content.

## Credit where it's due

The actual system - LangGraph agent, parent/child chunking, hybrid dense+sparse retrieval, the whole Gradio app - is [Giovanni Pasqualino's agentic-rag-for-dummies](https://github.com/GiovanniPasq/agentic-rag-for-dummies). MIT licensed, so I forked it and adapted it rather than pretending I built the retrieval pipeline myself. All the original code, notebooks, docs and license are still in here as-is. Go check out the original repo if you want the full writeup of how the architecture works, I'm not going to repeat that here.

## What I actually changed

- Added `sample_docs/` with two of my own interview-prep PDFs (`30_RAG_Interview_Questions_AmanAI_Lab.pdf`, `30_LLM_Interview_Questions_AmanAI_Lab.pdf`) so there's something concrete to upload and query right away instead of having to go find your own PDF first.
- Tuned the chunking config in `project/config.py` (`CHILD_CHUNK_SIZE`, `CHILD_CHUNK_OVERLAP`, `MIN_PARENT_SIZE`) down from the repo defaults. Those PDFs are short Q&A pairs, not long-form articles, and the project's own docs (see `project/README.md`) call out that FAQ-style content wants smaller child chunks (~300 tokens) than the 500 the repo ships with by default. Left `MAX_PARENT_SIZE` alone.

Everything else - the graph logic, the retrieval tools, the UI - is untouched.

## Running it

Same as the original project:

```
pip install -r requirements.txt
python project/app.py
```

Needs Ollama running locally for the default model (`granite4.1:8b`) - or swap providers per the instructions in `project/README.md` if you'd rather use OpenAI/Anthropic/Google. Open `http://localhost:7860`, upload the PDFs from `sample_docs/`, and ask it something like "what's the difference between dense and sparse retrieval" and see what it pulls back.

Full disclosure: I adapted the config and added the sample docs, but I haven't run a full end-to-end pass with Ollama on this machine yet to confirm output quality - treat the chunking numbers above as a reasoned starting point based on the project's own sizing guidance, not something I've empirically validated on this exact content. If you try it and the retrieval quality is off, that's the first knob I'd revisit.
