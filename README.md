<div align="center">

# 📈 AAPL Equity Research Assistant

**A research assistant that pulls live market data for a stock, runs technical analysis, scores recent news with an LLM and produces a formatted research brief.**

<br/>

[![Open In Colab](https://img.shields.io/badge/Open%20in%20Google%20Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=black)](https://drive.google.com/file/d/1UT_xtN8FOKsSl6VYFGNLeq9ZxsXVONFy/view?usp=sharing)&nbsp;&nbsp;
[![Get Groq API Key](https://img.shields.io/badge/Get%20Groq%20API%20Key-Free-FF4500?style=for-the-badge&logo=groq&logoColor=white)](https://console.groq.com/keys)

<br/>

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Groq](https://img.shields.io/badge/Groq-openai%2Fgpt--oss--20b-FF4500?style=flat-square&logo=groq&logoColor=white)](https://console.groq.com)
[![Pydantic](https://img.shields.io/badge/Pydantic-v2-E92063?style=flat-square&logo=pydantic&logoColor=white)](https://docs.pydantic.dev)
[![pandas](https://img.shields.io/badge/pandas-2.2-150458?style=flat-square&logo=pandas&logoColor=white)](https://pandas.pydata.org)
[![yfinance](https://img.shields.io/badge/yfinance-0.2.66-8E44AD?style=flat-square&logo=yahoo&logoColor=white)](https://pypi.org/project/yfinance/)
[![matplotlib](https://img.shields.io/badge/matplotlib-charting-11557C?style=flat-square&logo=python&logoColor=white)](https://matplotlib.org)
[![License](https://img.shields.io/badge/License-MIT-2ECC71?style=flat-square&logoColor=white)](LICENSE)

</div>

---

## What it does

The notebook fetches 617 trading days of AAPL price data from Yahoo Finance (March 2024 to August 2026), computes SMA-50, SMA-200, RSI-14, MACD(12,26,9) and Bollinger Bands from scratch in pandas. It retrieves ten recent headlines via yfinance and scores each one with `openai/gpt-oss-20b` via the Groq API, getting back a validated JSON object per headline. Those scores feed into a confidence-weighted sentiment aggregate (+0.42, positive), which gets passed alongside all the indicator values to a second LLM call that reasons over how the signals interact and returns a Buy, Hold, or Sell verdict. Every LLM response goes through Pydantic v2 validation before being used, with failures handled gracefully to prevent crashes. The final output is a styled HTML report rendered inline in the notebook.

---

## Output

Running all 15 cells produces:

| # | Output |
|---|--------|
| 1 | A summary dictionary with current price, 52-week high/low, P/E ratio, YTD return and a four-component composite momentum signal |
| 2 | Sentiment scores for each of the 10 headlines with a confidence-weighted aggregate sentiment score |
| 3 | A confidence supported trading signal with a concise justification addressing the golden cross setup, RSI headroom, MACD histogram confirmation and Bollinger Band placement |
| 4 | A two-panel dark-theme price chart embedded as a base64 PNG |
| 5 | The full HTML brief rendered in the notebook output, including company snapshot grid, chart, indicator table, scored headlines, signal verdict and a risk disclaimer |

---

## Stack

| Library | Role |
|---|---|
| `yfinance` | OHLCV data and news headlines, with DuckDuckGo as headline fallback |
| `pandas` / `numpy` | All five indicators computed from formula, no TA-Lib |
| `groq` SDK | Inference via `openai/gpt-oss-20b`; falls back to live model list if the primary is unavailable |
| `pydantic` v2 | `HeadlineSentiment` and `TradingSignal` models validate every LLM response |
| `matplotlib` | Two-panel chart encoded in memory as base64 PNG |
| `duckduckgo-search` | News fallback when yfinance returns fewer than 10 headlines |

---

## Setup

### 1 · Open in Colab

[![Open In Colab](https://img.shields.io/badge/Open%20Notebook%20in%20Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=black)](https://drive.google.com/file/d/1E5F7ygxwN10AFcD0Vl14QXAW3an_oQpu/view?usp=sharing)

### 2 · Add your Groq API key

[![Get Free API Key](https://img.shields.io/badge/Get%20Free%20Groq%20API%20Key-FF4500?style=for-the-badge&logo=groq&logoColor=white)](https://console.groq.com/keys)

In Colab, open the **Secrets** panel and add a secret named `GROQ_API_KEY`. The notebook reads it with `userdata.get("GROQ_API_KEY")` and also checks `os.getenv("GROQ_API_KEY")` if you run it locally.

### 3 · Run cells in order

| Cells | Purpose |
|---|---|
| `Cell 1` | Install dependencies |
| `Cells 2–4` | Load config, validate the API key and run imports |
| `Cells 5–6` | Define prompt constants and Pydantic models |
| `Cells 7–10` | Data pipeline |
| `Cells 11–13` | LLM calls |
| `Cells 14–15` | Generate the chart and render the report |

---

## Repository structure

```
task1_financial/
├── Task_1_Sayuni_Ellepola.ipynb   # Main notebook with all cell outputs
../CITATIONS.md                    # AI tool usage log
../REFLECTION.md                   # Design decisions and trade-offs
../README.md                       # Task overview
```

---
