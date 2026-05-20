# ATR Volatility Alert — MQL4 Script

A MetaTrader 4 script that monitors **Average True Range (ATR)** in real time and fires alerts on two distinct volatility events: sudden ATR spikes relative to the previous value, and bar-to-bar price moves that exceed a multiple of the current ATR.

---

## Overview

This script provides two complementary volatility signals using a single ATR calculation. The spike detector catches sudden expansions in market volatility by comparing the current ATR to the prior cycle's value. The threshold detector flags abnormally large individual price moves by measuring whether the bar-to-bar close change exceeds `ATRMultiplier × ATR`. Together they give early warning of both building and immediate volatility events.

---

## Features

- **Dual-condition volatility detection** — ATR spike and price-beyond-ATR-threshold
- **Persistent ATR tracking** — `PrevATR` global variable retains previous cycle's reading
- **Three notification channels:** sound alert, email, and mobile push
- **Configurable symbol, timeframe, period, multiplier, and spike threshold**
- **Lightweight loop** — polls once per minute (`Sleep(60000)`)
- Logs all alert events to the MT4 **Experts** tab

---

## How It Works

1. Every minute, `iATR()` computes the current ATR over `ATRPeriod` bars
2. `iClose()` fetches the current and previous bar's closing prices
3. Two conditions are evaluated independently:
   - **Volatility Spike:** `currentATR > PrevATR × VolatilitySpikeThreshold` → **Volatility Spike Detected**
   - **Price Threshold:** `|currentClose − prevClose| > currentATR × ATRMultiplier` → **Price Moved Beyond ATR Threshold**
4. `PrevATR` is updated at the end of each cycle for the next comparison

---

## Input Parameters

| Parameter                 | Type            | Default     | Description                                                   |
|---------------------------|-----------------|-------------|---------------------------------------------------------------|
| `TradeSymbol`             | string          | `EURUSD`    | Symbol for analysis                                           |
| `Timeframe`               | ENUM_TIMEFRAMES | `PERIOD_H1` | Timeframe for analysis                                        |
| `ATRPeriod`               | int             | `14`        | ATR lookback period                                           |
| `ATRMultiplier`           | double          | `2.0`       | Multiplier for price-beyond-ATR threshold                     |
| `VolatilitySpikeThreshold`| double          | `1.5`       | Ratio of current to previous ATR required to trigger a spike  |
| `EnableAlerts`            | bool            | `true`      | Fire an on-screen/sound alert                                 |
| `EnableEmail`             | bool            | `false`     | Send an email notification                                    |
| `EnablePush`              | bool            | `false`     | Send a mobile push notification                               |

---

## Alert Message Format

```
Volatility Spike Detected on EURUSD (Timeframe: PERIOD_H1)
ATR Value: 0.00085
```

---

## Installation

1. Copy `Average_True_Range__ATR__001.mq4` to `MQL4/Scripts/` in your MT4 data folder
2. Compile in MetaEditor (F7)
3. Drag onto any chart from Navigator → Scripts
4. Configure inputs and click **OK**

---

## Requirements

- MetaTrader 4 (`#property strict` compatible build)
- MQL4 compiler (MetaEditor)

---

## License

MIT License

Copyright (c) 2026

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
