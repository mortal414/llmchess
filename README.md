# LLM Chess

My experiments with language models playing chess, with exact board state, strict move validation, PGN replays, raw responses, and an API budget ledger.

**Status:** I made this for fun and learning. I'm not currently using or developing it. The repository also retains material from the GPT Learning video idea, which did not become an ongoing series.

The repository has two interfaces:

- **`llmchess-lab`** is the experiment runner documented in the [saved chess protocol](docs/episode-02/PROTOCOL.md). It calls the official OpenAI Responses API and records model-specific token counts, usage, settings, and every move.
- **The Flask / Next.js app** is the older interactive control room. It supports direct OpenAI or an explicitly configured OpenAI-compatible gateway. For an editable prompt-graph playground, see [llm-chess-lite-2](https://github.com/mortal414/llm-chess-lite-2).

The saved experiment is titled **GPT announced checkmate. The king moved.** Read the [results and method](docs/episode-02/REPORT.md), inspect [all seven games](evidence/episode-02/games), or replay the 239-request [evidence audit](evidence/episode-02/audit.json). The retained film source has two native layouts. This is source material from a hobby experiment, not a claim that an episode was published.

## Experiment runner

Python 3.11+ and a local Stockfish installation are required for engine games. Stockfish is not needed for the 12-position puzzle suite or offline tests.

```sh
python3 -m venv .venv
source .venv/bin/activate
pip install -e '.[dev]'
cp .env.example .env
# Add your OPENAI_API_KEY. Never commit it.

# Audit the saved experiment without making API calls:
python -m llmchess_lab.analyze

# For a NEW paid run, use a fresh directory and review the generated protocol:
llmchess-lab prepare --out runs/my-chess-run
llmchess-lab puzzles --out runs/my-chess-run
llmchess-lab games --out runs/my-chess-run --engine /path/to/stockfish
```

The supplied episode protocol caps all experiment requests at **$17**, leaving $3 of the episode's $20 budget for narration and QA. These are explicit planning caps, not a statement that running the example is free. Calls reserve their maximum input/output allowance before dispatch. Actual API usage reduces the reservation afterward. Cache discounts are not assumed. An uncertain request retains its reservation and is never retried automatically. Use a fresh `--out` directory for a new experiment; do not overwrite saved evidence.

The supplied protocol uses GPT-6 Astra, GPT-5.6 Sol and GPT-5.6 Luna at low reasoning effort, 2,048 total output tokens. Prompts provide FEN, an ASCII board, move history, and an unranked legal-move list. Responses contain an exact JSON move plus a short public comment. No engine hints, repairs, or substituted moves. A legal move is not necessarily a good move.

Files include a fixed protocol, deterministic cases, raw provider responses, usage, a budget ledger, per-game JSON, and PGN. Rules draws and checkmates remain separate from invalid replies, transport errors, and unfinished games. The small constructed puzzle set, three primary games and four separately planned follow-ups do **not** support an Elo rating or broad model ranking. The analyzer audits this saved experiment, including its source hashes and follow-up manifest; it is not a generic benchmark grader.

## Interactive control room

```sh
pip install -e .
python server.py
# In another terminal:
cd ui
npm ci
printf 'NEXT_PUBLIC_API_BASE=http://127.0.0.1:8000\n' > .env.local
npm run dev
```

Open http://127.0.0.1:3000. The API binds to localhost and accepts browser requests from the local UI. `OPENAI_API_KEY` selects the direct OpenAI endpoint. For a gateway, explicitly set `LLMCHESS_LLM_BASE_URL` and `LLMCHESS_LLM_API_KEY`; model route names must match that gateway. The interactive legacy UI does not use the laboratory's dollar ledger. Use the lab for measured, budgeted experiments.

Mock data is off by default and is only shown when `NEXT_PUBLIC_USE_MOCKS=true`; a visible banner identifies synthetic demo data. A failed backend cannot silently become a mock experiment result.

## Validation and film source

```sh
python -m pytest -q
cd ui
npm run lint
npm run build
npm audit
```

The [production notes](docs/episode-02/PRODUCTION.md) specify the short film's pacing, phone composition, exact quotes, and readability requirements. Original procedural chess artwork and production code live in `video/chess/`; the video extras require FFmpeg, Pillow, and NumPy. Stock voices are explicitly synthetic. The intended public files exclude engine binaries, API keys, and private project settings.

The original gateway configuration and notation reference remain in [docs/configuration.md](docs/configuration.md) and [docs/chess-text-representation.md](docs/chess-text-representation.md).
