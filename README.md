# ZARQ Track Record

Daily SHA-256 hash-chained crypto risk signals from [ZARQ](https://zarq.ai).

## What is this?

Every day, ZARQ generates risk signals for 205 crypto tokens. Each daily entry includes:

- **Trust Score** (0-100) and **Moody's-style rating** (Aaa-D) for each token
- **Distance-to-Default** (DtD) — 0-5 scale measuring structural health
- **Structural weakness count** — number of active risk signals
- **Crash probability** — model-estimated likelihood of >50% drawdown
- **ZARQ Warning** flag — whether the token has a structural collapse/stress alert
- **Price at signal date** — for later verification of predictions

Each entry is SHA-256 hashed and chain-linked to the previous entry, creating an **immutable, publicly verifiable record**.

## Verification

Every entry contains a `hash` field (SHA-256 of the signal data) and a `previous_hash` field linking to the prior day.

To verify the chain:

```bash
# Verify a single entry's hash
cat signals/2026/2026-03-07.json | python3 -c "
import sys, json, hashlib
d = json.load(sys.stdin)
stored_hash = d.pop('hash')
prev = d.pop('previous_hash', None)
generated = d.pop('generated_at', None)
computed = hashlib.sha256(json.dumps(d, sort_keys=True).encode()).hexdigest()
print(f'Stored:   {stored_hash}')
print(f'Computed: {computed}')
print(f'Match: {stored_hash == computed}')
"
```

## Why this matters

- **No hindsight bias** — signals are published daily before outcomes are known
- **Immutable** — SHA-256 hash chain prevents retroactive modification
- **Publicly verifiable** — anyone can audit the full history
- **Track record from day 1** — building credibility through transparency

## Stats

- **Coverage:** 205 tokens
- **Start date:** 2026-03-07
- **Signal types:** Structural Collapse (CRITICAL), Structural Stress (WARNING), Recovery
- **Out-of-sample performance:** 113/113 token deaths detected, 98% precision

## Links

- [ZARQ API](https://zarq.ai/v1/check/bitcoin) — free, no auth
- [API Docs](https://zarq.ai/zarq/docs)
- [MCP Server](https://smithery.ai/server/agentidx/zarq-risk) — 11 tools for AI agents
- [PyPI](https://pypi.org/project/zarq-langchain/) — LangChain integration
- [npm](https://www.npmjs.com/package/@zarq/elizaos-plugin) — ElizaOS plugin

## License

Data is published under CC BY 4.0. Attribution: ZARQ (zarq.ai).
