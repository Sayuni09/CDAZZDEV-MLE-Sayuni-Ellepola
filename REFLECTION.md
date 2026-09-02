## Task 1 — Financial AI LLM-Powered Equity Research Assistant

---

### Architectural Decisions


**Ticker selection.** I chose AAPL because the current news cycle creates a genuine leadership transition that produces heterogeneous sentiment across headlines, making sentiment aggregation more analytically interesting to stress-test than a ticker with uniformly one-directional coverage.

**Model selection and dynamic validation.**  Rather than hardcoding a model string, Cell 3 implements a two-stage probing loop. It first attempts the preferred model list via live probe requests, then falls back to fetching the full Groq models endpoint and filtering out audio, vision and guard models. Temperature is fixed at 0.1, trading creativity for near-deterministic JSON output and stabilising downstream Pydantic validation.

**Indicator computation from first principles.** All five indicators use pandas and numpy only. The RSI uses Wilder's exponential smoothing rather than a simple rolling mean and the MACD uses Appel's EMA with a different alpha that is not interchangeable with Wilder smoothing. 

**Prompt separation.** All prompt text lives as module-level string constants in Cell 5, referenced by name from business logic functions. The signal system prompt enumerates four reasoning dimensions covering SMA and RSI alignment, MACD confirmation, Bollinger Band implication and overall synthesis because without this instruction, smaller open-weight models restate individual indicator values rather than reason across their interactions. 

**Pydantic v2 validation and fallback design.** Both schemas use field validators with the classmethod decorator. The shared LLM helper catches JSON parse errors, Pydantic validation errors and generic exceptions separately, logging the raw response in every case. Fallback objects are pre-constructed Pydantic instances so downstream code never needs type-based branching. The signal fallback returns Hold with confidence zero, preventing a validation failure from triggering a spurious directional trade.

**HTML report rendering.** The chart is encoded in memory via a byte buffer and embedded as a base64 data URI, making the report self-contained with no dependency on external file paths or hosted images.

---

### What I would improve with more time

The per-headline sentiment calls are serialised with a half-second delay, adding roughly five seconds for ten headlines. I would batch all headlines into a single prompt using a JSON array schema and request one structured response for all items, reducing wall-clock time by around 80 percent.

The momentum signal is a linear weighted sum over four binary components with the bullish threshold set heuristically. With more time I would calibrate this against historical signal-to-return data, or replace the composite by asking the LLM to score momentum on a continuous scale from the indicator values directly.

News retrieval returns exactly ten headlines because yfinance returned exactly ten items on this run. A production implementation would draw from multiple sources including SEC EDGAR RSS feeds and deduplicate by semantic similarity rather than exact string matching.

---

### Limitations 

The Groq free tier token limits made serialising sentiment calls necessary. This was a concession to infrastructure constraints rather than an architectural preference and would be the first thing to change in production.

The trailing PE of 37.25 from yfinance is plausible for AAPL but can lag by up to one earnings cycle. A production pipeline would source this from a more timely provider.

Using a smaller open-weight model rather than a frontier model introduces a quality ceiling on the justification text. The model follows the structured output format reliably, but its reasoning depth is shallower than a GPT-4-class model would produce. The architecture isolates this cleanly since swapping to a stronger model only requires updating the model identifier in Cell 2.
