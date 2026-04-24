# Kalshi Mirror Trader

> Mirrors the positions of selected Kalshi traders automatically. When a tracked trader enters or exits a market, the bot replicates the move in your account at a proportional size.

![preview_mirror trader position sync](https://github.com/user-attachments/assets/c4df9074-f821-45fb-acd4-a869573695ec)

---


*Last updated: April 2026*

## What is Kalshi Mirror Trader?

Kalshi Mirror Trader monitors the position activity of traders you select and hands-free replicates their Kalshi trades in your account. It detects new entries and exits in real time, scales position sizes to match your configured allocation, and executes through the Kalshi CLOB API.

You define who to mirror and how much capital to allocate. The bot handles the timing.

---

## Download

| Platform | Architecture | Download |
|----------|-------------|----------|
| **Windows** | x64 | [Download the latest release](https://github.com/geckodevil99/kalshi-mirror-trader/releases) |

---

## How Mirroring Works

| Step | What Happens |
|------|-------------|
| 1. Track | Bot monitors position changes for your selected trader list |
| 2. Detect | New entry or exit detected within seconds |
| 3. Scale | Position sized to your configured allocation ratio |
| 4. Execute | Order placed via Kalshi CLOB API |
| 5. Log | Trade recorded with source trader and your execution price |

---

## Engine Features

* **Real-time position sync** - detects trader activity within seconds
* **Proportional scaling** - mirrors at your configured % of the source trade size
* **Multi-trader support** - mirror up to 10 traders simultaneously
* **Exit mirroring** - replicates exits, not just entries
* **Delay control** - optional lag to avoid detection
* **Daily cap** - set maximum USD deployed per day per trader
* **Telegram alerts** - notified on every mirrored entry and exit

---

## Two Ways to Run It

| | Windows App | Python Bot |
|---|---|---|
| **Setup** | Double-click | `pip install` + config |
| **Trader list** | Config-based | Dynamic updates |
| **Scale ratio** | Per-trader setting | Custom logic |
| **Config** | `config.toml` | Direct code access |
| **Alerts** | Dashboard + Telegram | JSON + Telegram |

## Quick Start

```
# 1. Download from Releases
# 2. Edit config.toml - add trader usernames and allocation %
# 3. Run Mirror Trader - tracking and replication starts immediately
```

### Python

```bash
cd kalshi-mirror-trader/python
pip install -r requirements.txt
python kalshi-mirror-trader-v.1.4.14.py
```

---

## How It Works

![mirror trader pipeline](https://github.com/user-attachments/assets/abc8c558-7888-4026-a25f-670a34c8a3d1)

Four stages per cycle:

1. **Monitor** - polls position snapshots for each tracked trader
2. **Diff** - compares current positions to previous snapshot to detect changes
3. **Scale** - calculates your position size based on allocation ratio
4. **Execute** - places matching order on Kalshi CLOB

### Config Reference

```toml
[mirror]
traders = ["trader_a", "trader_b"]
scale_ratio = 0.5
min_position_usd = 10
max_position_usd = 200
daily_cap_usd = 500
delay_seconds = 2

[kalshi]
api_key = ""
api_secret = ""

[telegram]
bot_token = ""
chat_id = ""

[export]
trade_log_csv = "data/trades/mirror_trades.csv"
```

---

## Mirror Trade Log Format

```json
{
  "mirror_id": "mrr_20260321_027",
  "source_trader": "trader_a",
  "market": "INXD-26APR-B5200",
  "side": "YES",
  "source_size_usd": 180,
  "your_size_usd": 90,
  "entry_price": 0.41,
  "timestamp": "2026-03-21T11:22:08Z"
}
```

---

## Verified Live

**Configuration used:**
* 2 traders mirrored, scale ratio 0.5, daily cap $500

**Mirror trade executed:**

| | Details |
|---|---|
| Source trader | trader_a |
| Market | INXD-26APR-B5200 |
| Source position | YES $180 at 0.41 |
| Your position | YES $90 at 0.42 (2s lag) |
| Tx hash | 0x5c1e8a3f7b4d9e2c6a1f3e8b5d2c7f4a9e3b1d6c8f2a5e7b3d9c1f4a6e8b2d5 |

---

## Frequently Asked Questions

**What is Kalshi Mirror Trader?**
Kalshi Mirror Trader is an automated bot that replicates the trades of selected Kalshi traders in your account. When a tracked trader enters or exits a market, the bot mirrors the move at a proportional size.

**What is the difference between mirror trading and copy trading?**
Mirror trading replicates individual trades as they happen. Copy trading typically allocates a portfolio percentage to follow a trader over time. Kalshi Mirror Trader operates trade-by-trade.

**Can I mirror multiple traders at once?**
Yes. Add up to 10 traders to your watch list. Each is tracked independently with its own scale ratio and daily cap.

**Does it mirror exits too?**
Yes. When a tracked trader closes or reduces a position, the bot replicates the exit proportionally in your account.

**What is the delay setting for?**
The `delay_seconds` setting introduces a short lag between source trade detection and your execution.

**Is this a kalshi position sync bot?**
Yes. The core function is continuous position synchronization - your portfolio mirrors the selected trader's portfolio at your configured scale.

**What happens if a market has low liquidity?**
If your mirrored order cannot fill at a reasonable price, the bot logs the failure and skips that trade. It does not chase the order across the spread.

---

## Use Cases

- **Kalshi mirror trading** - on its own replicate top trader positions without manual monitoring
- **Kalshi follow trader bot** - follow high-performing Kalshi traders with proportional allocation
- **Kalshi trade sync** - keep your positions synchronized with a selected trader in real time
- **Kalshi shadow trader** - silent position mirroring with configurable delay and scaling
- **Kalshi copy mirror** - replicate both entries and exits across all market categories

---

## Repository Structure

```
kalshi-mirror-trader/
+-- kalshi-mirror-trader-v.1.4.14.exe
+-- config.toml
+-- data/
|   +-- trades/
|   +-- snapshots/
|   +-- logs/
|   +-- dll/
+-- python/
|   +-- src/
|   |   +-- tracker.py
|   |   +-- differ.py
|   |   +-- executor.py
|   +-- requirements.txt
+-- README.md
```

---

## Requirements

```
python-dotenv, typer[all], httpx, kalshi-python
```

* Kalshi account with API trading access
* Telegram bot token (for alerts)

---

*More signals. Less noise.*
