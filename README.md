# 🛡️ Guardian Agent

> AI agents that execute blockchain transactions — without ever seeing the private key.

## Live Demo

**[https://izmiradami.github.io/guardian-agent](https://izmiradami.github.io/guardian-agent)**

## The Problem

Today, every AI agent that needs to sign a transaction gets direct access to the private key. This means:

- Keys are scattered across agent memory
- One compromised agent = full wallet access
- No enforcement layer between AI intent and execution

## The Solution

Guardian Agent sits between the AI and the wallet. The agent **requests** a transaction. Guardian **decides** if it's allowed. The key is **never exposed**.

```
AI Agent → Interface → Policy Engine → Signer → Vault
                            ↓
                      (key never leaves here)
```

## How It Works

1. **Guardian Agent** sends a sign request `{ token, amount, recipient }`
2. **Interface** validates the request schema
3. **Policy Engine** enforces rules — max spend, allowed tokens, simulation
4. **Signer** asks the vault for a one-time sign operation
5. **Vault** decrypts the key in an isolated memory region, signs, then immediately zeroizes
6. **Agent receives** only the transaction hash — never the key

## Key Properties

| Property | Value |
|---|---|
| Vault encryption | AES-256-GCM |
| Key derivation | Argon2id |
| Signing curve | secp256k1 |
| Agent key access | ❌ DENIED |
| Architecture | Zero-trust |

## Policy Engine

User-configurable rules enforced before any signing:

- **Max spend** — reject if transaction exceeds USD limit
- **Token allowlist** — only permitted tokens can be used
- **Simulation required** — dry-run before broadcasting

If any policy fails, the vault is never touched.

## Demo

Open `index.html` in any browser — no backend, no dependencies.

```bash
git clone https://github.com/izmiradami/guardian-agent
cd guardian-agent
open index.html
```

Try these scenarios:

- ✅ **Happy path** — ETH, $150, max $200 → transaction completes
- ❌ **Exceed limit** — change amount to $350 → rejected at policy layer
- ❌ **Wrong token** — switch to USDC → rejected, vault never opened

## Architecture

Built on top of the [Open Wallet Standard (OWS)](https://github.com/openwalletfoundation) infrastructure for local-first encrypted key storage.

Guardian Agent adds:
- Policy enforcement layer
- Agent-facing interface
- Simulation pipeline
- Audit log streaming

## Pitch

> "Today, every tool manages private keys differently — scattered, unencrypted, and unsafe. Guardian Agent fixes this by introducing a local-first encrypted vault with a policy engine. An AI agent can request transactions, but every action is enforced by policies — and the private key is never exposed, only temporarily decrypted and immediately wiped."

## License

MIT
