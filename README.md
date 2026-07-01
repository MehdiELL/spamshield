<div align="center">

# 📮 SpamShield

### AI spam filter on GenLayer

[![GenLayer](https://img.shields.io/badge/GenLayer-Bradbury-ff4d6d)](https://genlayer.com)
[![chainId](https://img.shields.io/badge/chainId-4221-4dd0e1)](https://docs.genlayer.com/developers/networks)
[![Intelligent Contract](https://img.shields.io/badge/intelligent%20contract-Python%20GenVM-8a63d2)](https://docs.genlayer.com/developers/intelligent-contracts/introduction)
[![License](https://img.shields.io/badge/license-MIT-2dd4bf)](LICENSE)

</div>

---

## What is this

Submit text; GenLayer validators independently evaluate it with an LLM, agree on a label and score by consensus, and write the result on chain. AI spam filter.

**Deployed contract (Testnet Bradbury):** [`0x0D8707Ba8075D90E678236270b2Ba98bd29FAcD6`](https://explorer-bradbury.genlayer.com/address/0x0D8707Ba8075D90E678236270b2Ba98bd29FAcD6)

## Why GenLayer

This is a subjective judgment over real-world text input - exactly what a
deterministic VM cannot do. GenLayer runs the LLM evaluation **inside consensus**, so the
verdict gets the same Byzantine-fault tolerance a normal chain gives to arithmetic.

## How it works

1. **`submit(text)`** - submit text to be evaluated.
2. **`evaluate(id)`** - each validator classifies the content
   (one of: ham, promotional, spam) and scores it 0-100, then must **agree** on the label and a similar
   score via the Equivalence Principle.
3. The **label, score, and reasoning** are written on chain.

## The Intelligent Contract

`contracts/spamshield.py` targets the GenVM Python runner (pinned by hash):

- **Integers only** - the spamminess score is `u8` (0-100); no floats.
- **Coarse-band equivalence** - validators must agree on the label and land within **+/-25**
  score, so heterogeneous LLMs converge instead of returning `Undetermined`.
- **Prompt-injection defense** - the input is treated as untrusted data.
- **Low-RPC reads** - `get_info()` / `list_items()` return state in one call.

Public methods: `submit`, `evaluate`, `get_item`, `list_items`, `get_info`.

## Develop & test

```bash
pip install -r requirements.txt        # Python 3.12+
genvm-lint check contracts/spamshield.py
pytest tests/direct/ -v
```

## Deploy

```bash
npm install -g genlayer
genlayer network set testnet-bradbury
bash deploy/deploy.sh
```

## Network

| | |
| --- | --- |
| Network | GenLayer Bradbury testnet |
| Chain ID | 4221 |
| RPC | https://rpc-bradbury.genlayer.com |
| Explorer | https://explorer-bradbury.genlayer.com |
| Faucet | https://testnet-faucet.genlayer.foundation |

## License

[MIT](LICENSE) (c) 2026 MehdiELL.
