# LLM Chess Lite

A local chess playground I made for fun, with an editable prompt graph and visible per-request conversations between a human and an OpenAI model.

**Status:** I'm not currently using or developing this project.

The default graph sends one request, accepts one exact legal UCI move, and stops on a bad reply. It never substitutes a move or silently repairs the answer. The default is GPT-5.6 Luna, low reasoning effort, 2,048 total output tokens. Change the model in Configuration.

## Run

Node 22.12+ is required.

```sh
npm ci
cp .env.example .env
# Add OPENAI_API_KEY to .env; it is excluded from Git.
npm run dev
```

Client: http://127.0.0.1:5173 · API: http://127.0.0.1:8787

```sh
npm test
npm run build
npm audit
```

The key stays on the server. Automatic SDK retries are disabled. A provider failure stops the move; an uncertain request may still be billed. This interactive playground has no dollar budget ledger. For budgeted experiments, token-count preflight, immutable responses, PGN export, and saved experiment evidence, use [llmchess-lab](https://github.com/mortal414/llmchess).

## Prompt graph

The saved graph is snapshotted when a game starts. Configuration edits affect the next game after saving. Existing saved custom graphs are preserved: click **New** in Configuration to create a system using the new strict graph.

Template variables include `{fen}`, `{sideToMove}`, `{legalMovesText}`, `{sanHistory}`, `{uciHistory}`, `{moveNumber}`, `{lastMove}`, `{candidateMove}`, and `{node.parse_move.output.move}`.

Explicit `firstLegalMove` nodes remain available for custom playground graphs and are labeled as fallbacks in the returned trace. They are absent from the default and must never be scored as a model's chess move. The parser now distinguishes long and short castling and rejects commentary containing several candidate moves.

## Related experiment

The [saved companion experiment](https://github.com/mortal414/llmchess/tree/main/docs/episode-02) compares the models configured at the time on small endgames and records complete games. Its `episode-02` folder name comes from the GPT Learning video idea, which did not become an ongoing series. This UI is for interactive exploration, not the source of those experiment scores.
