# ============================================================
# Task     : Task 1 - Financial AI LLM-Powered Equity Research Assistant
# ============================================================

# AI Tool Usage

# ============================================================

# AI-ASSISTED: Claude (claude-sonnet-4-6),
# Prompt: 'I am building an LLM-powered equity research assistant in a Colab
# notebook. It needs to fetch stock data, compute technical indicators, pull
# news headlines, run sentiment analysis through an LLM and output a
# structured report.',
# Date: 2026-09-01

# ============================================================

Cell 1 — Package Installation

# AI-ASSISTED: Claude (claude-sonnet-4-6),
# Prompt: 'Write a single pip install cell for a Colab notebook that installs
# groq, yfinance, pydantic, duckduckgo-search, and requests. Pin versions that
# are compatible with each other and print the installed versions to confirm.',
# Date: 2026-09-01

# ============================================================

Cell 2 — Configuration Constants

# AI-ASSISTED: Claude (claude-sonnet-4-6),
# Prompt: 'Define all pipeline configuration constants including indicator
# windows, RSI/MACD/Bollinger parameters, momentum scoring weights and Groq
# model fallback list as named module-level variables with no magic numbers',
# Date: 2026-09-01

# ============================================================

Cell 3 - API Key Loading and Groq Model Validation

# AI-ASSISTED: Claude (claude-sonnet-4-6),
# Prompt: 'Write me a function that loads a Groq API key from Colab Secrets
# first and falls back to an environment variable if Colab Secrets is not
# available. Then write a model validation function that tries each model in
# a preferred list one by one and if all fail it fetches the live model list
# from the Groq API and tries every text-generation model it finds there.
# Return the first model that responds successfully.',
# Date: 2026-09-01

# ============================================================

Cell 4 - Imports and Logging Configuration

# AI-ASSISTED: Claude (claude-sonnet-4-6),
# Prompt: 'Centralise all standard library and third-party imports for a
# financial data pipeline into a single cell, configure Python logging with
# timestamped INFO-level output and suppress yfinance FutureWarnings',
# Date: 2026-09-01


# ============================================================

Cell 5 - Prompt Constants

# AI-ASSISTED: Claude (claude-sonnet-4-6),
# Prompt: 'Write two sets of prompt constants for a financial LLM pipeline.
# The first is for per-headline sentiment analysis and must return JSON with
# exactly four fields as headline, sentiment, confidence and brief_reason.
# The second is for a trading signal and must instruct the model to reason
# over how SMA crossover, RSI, MACD histogram and Bollinger Band position
# interact and confirm or contradict each other, then return JSON with signal,
# confidence and justification. Define all prompts as module-level string
# constants, never inline.',
# Date: 2026-09-01

# ============================================================

Cell 6 - Pydantic v2 Validation Models and Fallback Objects

# AI-ASSISTED: Claude (claude-sonnet-4-6),
# Prompt: 'Write two Pydantic v2 models: HeadlineSentiment with fields
# headline, sentiment, confidence and brief_reason, and TradingSignal with
# fields signal, confidence and justification. Add field validators that
# enforce sentiment to be positive negative or neutral, signal to be Buy Hold
# or Sell and confidence to be a float between 0 and 1. Also create fallback
# instances of each model that get returned when validation fails so the
# pipeline never raises an exception.',
# Date: 2026-09-01

# ============================================================

Cell 7 — OHLCV Data Fetch

# AI-ASSISTED: Claude (claude-sonnet-4-6),
# Prompt: 'Write an OHLCV fetch function using yfinance that computes the
# start date dynamically from today minus a configurable number of days so
# there are no hardcoded date strings. Try the bulk download method first
# and fall back to the single-ticker history method if it returns empty.
# Flatten any MultiIndex columns, forward-fill price NaNs, zero-fill volume
# NaNs and raise a RuntimeError with a useful message if both methods fail.',
# Date: 2026-09-01

# ============================================================

Cell 8 - Technical Indicators from First Principles

# AI-ASSISTED: Claude (claude-sonnet-4-6),
# Prompt: 'Implement these five technical indicators using only pandas and
# numpy, no TA-Lib. SMA with a configurable window. RSI-14 using Wilder
# smoothing where alpha equals 1 divided by period, not the standard EMA
# formula. MACD with fast 12 slow 26 signal 9 using standard Appel EMA.
# Bollinger Bands with window 20 and 2 standard deviations using sample
# standard deviation with ddof 1. Wrap them all in a builder function that
# appends every indicator as a new column on the OHLCV dataframe.',
# Date: 2026-09-01


# ============================================================

Cell 9 - News Retrieval with DuckDuckGo Fallback

# AI-ASSISTED: Claude (claude-sonnet-4-6),
# Prompt: 'Write a news retrieval pipeline that fetches headlines from the
# yfinance news endpoint first. Handle both the old response format where
# the title is at item title and the newer nested format where it is at
# item content title. If fewer than 10 headlines come back, fall back to
# DuckDuckGo news search to top up the list. Deduplicate while preserving
# order and raise a RuntimeError only if the total from both sources is zero.',
# Date: 2026-09-01

# ============================================================

Cell 10 — Summary Dictionary and Composite Momentum Signal

# AI-ASSISTED: Claude (claude-sonnet-4-6),
# Prompt: 'Implement a weighted composite momentum signal scoring SMA crossover,
# RSI contrarian position, MACD histogram direction and Bollinger Band position
# each as +1/0/-1 with configurable weights summing to 1.0, combined with a
# summary dictionary builder that computes 52-week high and low over a trailing
# WEEK_52_DAYS window, fetches P/E ratio from yfinance.info with graceful None
# fallback and calculates YTD return from a dynamically derived January 1st
# start date',
# Date: 2026-09-01

# ============================================================

Cell 11 — Groq API Helper with JSON Extraction and Pydantic Validation

# AI-ASSISTED: Claude (claude-sonnet-4-6),
# Prompt: 'Write a reusable Groq API call function that takes a system prompt,
# a user prompt, a Pydantic model class and a fallback object. It must call
# the API with the system and user roles correctly separated, strip any markdown
# code fences from the response before parsing, parse the JSON, validate it
# against the Pydantic model and return the fallback object if anything fails.
# Log the raw response text whenever a failure occurs.',
# Date: 2026-09-01


# ============================================================

Cell 12 — Per-Headline Sentiment Analysis and Aggregation

# AI-ASSISTED: Claude (claude-sonnet-4-6),
# Prompt: 'Write a sentiment analysis loop that calls the Groq helper once per
# headline and collects the results. After all calls aggregate using a
# confidence-weighted mean where positive maps to plus 1, negative to minus 1
# and neutral to 0. Use a minimum weight of 0.1 per headline to avoid division
# by zero. Label the aggregate as positive above 0.10, negative below minus
# 0.10, neutral otherwise. Sleep 0.5 seconds between calls to stay within
# rate limits.',
# Date: 2026-09-01

# ============================================================

Cell 13 — Trading Signal Generation with Contextual Indicator Descriptions

# AI-ASSISTED: Claude (claude-sonnet-4-6),
# Prompt: 'Before sending indicator values to the LLM for a trading signal,
# convert each one into a short natural language description. For the SMA
# crossover describe whether it is a golden cross, death cross, early recovery,
# or transitional phase. For RSI describe overbought, oversold, bullish,
# bearish, or neutral with the actual value. For MACD histogram describe
# whether it is positive or negative and what that means for momentum. Compute
# the Bollinger Band percentage position as a number between 0 and 100. Then
# inject all of these into the signal prompt template and call the Groq helper.',
# Date: 2026-09-01

# ============================================================

Cell 14 — Matplotlib Chart Generation and Base64 Encoding

# AI-ASSISTED: Claude (claude-sonnet-4-6),
# Prompt: 'Generate a two-panel matplotlib figure with a dark background showing
# Close price overlaid with SMA-50, SMA-200 and Bollinger Band fill in the
# upper panel and RSI-14 with overbought/oversold fill regions in the lower
# panel.
# Save the
# figure to a BytesIO buffer and return it as a base64 encoded string. Do not
# write any file to disk.',
# Date: 2026-09-01

# ============================================================

Cell 15 — Styled HTML Equity Research Report

# AI-ASSISTED: Claude (claude-sonnet-4-6),
# Prompt: 'Compose a self-contained single-page HTML equity research brief with
# inline CSS covering five sections: Company Snapshot stat grid, Technical
# Outlook with embedded base64 chart and indicator table, News Sentiment Summary
# with confidence-weighted sentiment bar and top-3 headline cards, LLM
# Recommendation with signal verdict block and justification panel, and Risk
# Disclaimer.
# Style it as a professional financial
# document with a dark header and a clean white card body.',
# Date: 2026-09-01

# ============================================================

# Open Source References

# ============================================================

yfinance — OHLCV Fetch and Ticker.info

# SOURCE: yfinance library documentation and usage examples
# https://github.com/ranaroussi/yfinance
# Patterns adapted: yf.download() with auto_adjust=True, yf.Ticker().history(),
# yf.Ticker().news, yf.Ticker().info for P/E ratio retrieval

# ============================================================

yfinance - MultiIndex Column Flattening

# SOURCE: Known yfinance MultiIndex column behaviour documented in issues
# https://github.com/ranaroussi/yfinance/issues/1587
# Pattern: raw.columns.get_level_values(0) to flatten MultiIndex after
# yf.download() with multiple tickers or recent library versions

# ============================================================

Groq Python SDK - Chat Completions

# SOURCE: Groq Python SDK documentation
# https://console.groq.com/docs/openai
# https://github.com/groq/groq-python
# Patterns adapted: client.chat.completions.create() with model, messages,
# temperature and max_tokens parameters; /v1/models endpoint query


# ============================================================

Pydantic v2 - BaseModel with field_validator

# SOURCE: Pydantic v2 official documentation
# https://docs.pydantic.dev/latest/concepts/validators/
# Patterns adapted: @field_validator with @classmethod decorator, mode='before'
# vs default after-mode validation, model_validate() for dict ingestion

# ============================================================

RSI — Wilder Smoothing via pandas ewm

# SOURCE: Wilder, J.W. (1978). New Concepts in Technical Trading Systems.
# Implementation reference: https://school.stockcharts.com/doku.php?id=technical_indicators:relative_strength_index_rsi
# pandas ewm(com=period-1) corresponds to Wilder alpha=1/period, distinct from
# standard EMA alpha=2/(period+1); this distinction is documented in the
# pandas ewm docs at https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.ewm.html


# ============================================================

MACD - Appel EMA via pandas ewm(span=n)

# SOURCE: Appel, G. (2005). Technical Analysis: Power Tools for Active Investors.
# Implementation reference: https://school.stockcharts.com/doku.php?id=technical_indicators:moving_average_convergence_divergence_macd
# pandas ewm(span=n, adjust=False) gives alpha=2/(n+1), the standard Appel
# definition used by all major charting platforms

# ============================================================

Bollinger Bands — Sample Standard Deviation (ddof=1)

# SOURCE: Bollinger, J. (2001). Bollinger on Bollinger Bands.
# Implementation reference: https://school.stockcharts.com/doku.php?id=technical_indicators:bollinger_bands
# Bollinger's original specification uses sample standard deviation (ddof=1),
# implemented via pandas Series.rolling().std(ddof=1)

# ============================================================

duckduckgo-search — DDGS.news()

# SOURCE: duckduckgo-search library documentation
# https://github.com/deedy5/duckduckgo_search
# Pattern adapted: DDGS context manager with ddgs.news(query, max_results=n)
# returning a list of dicts with 'title' key

# ============================================================

matplotlib - Dark-theme Two-Panel Figure with Base64 Export

# SOURCE: matplotlib documentation
# https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.subplots.html
# https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.savefig.html
# Patterns adapted: gridspec_kw height_ratios for unequal panel sizing,
# fig.patch.set_facecolor for full-figure background, BytesIO in-memory
# PNG export, base64.b64encode for data URI embedding

# ============================================================

