# LLM Chess

Personal experiments with language models playing chess: exact board state, strict move validation, saved PGNs, raw responses, and API cost tracking.

**Inactive hobby project.** The saved games are a small experiment, not an Elo rating or general model ranking.

## Results

[Report](docs/episode-02/REPORT.md) · [Protocol](docs/episode-02/PROTOCOL.md) · [Seven saved games](evidence/episode-02/games) · [Response audit](evidence/episode-02/audit.json)

The `llmchess-lab` runner uses the OpenAI Responses API. Invalid moves stop the run rather than being repaired or replaced.

## Run

Python 3.11+ is required. Stockfish is optional for puzzle tests and required for engine games.

```sh
python3 -m venv .venv
source .venv/bin/activate
pip install -e '.[dev]'
cp .env.example .env
# Add OPENAI_API_KEY only for new API runs.

# Inspect saved evidence without API calls:
python -m llmchess_lab.analyze
```

[Usage](docs/usage.md) covers new experiments, cost limits, offline tests, and the older Flask / Next.js control room. New API runs cost money; the interactive control room does not use the lab's budget ledger.

For an editable prompt-graph UI, see [LLM Chess Lite](https://github.com/mortal414/llm-chess-lite-2).

## Video source

[Procedural chess artwork and film source](video/chess/) remain from the GPT Learning video idea. It did not become an ongoing series, and this repo does not claim the episode was published.
