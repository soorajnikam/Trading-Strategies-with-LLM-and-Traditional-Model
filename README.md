# Enhancing Trading Strategies with LLM and Traditional Models: A Business-Driven Sentiment Analysis Approach for Financial Market Forecasting

## Project Overview

This project presents a data-driven trading strategy for the EUR/USD currency pair, developed as part of the BIS Postgraduate Project at J.E. Cairnes School of Business and Economics, University of Galway (2024–25).

The system combines LLM-based sentiment analysis of financial news with LSTM-based time-series forecasting and technical indicators to generate hybrid trading signals and evaluate strategy performance over a six-year backtest period (2019–2025).


---

## Research Objective

To assess whether combining financial news sentiment with historical price data and technical indicators produces more accurate and profitable EUR/USD trading strategies than price-only or sentiment-only models.

---

## Key Features

- Financial news scraping from FXStreet using BeautifulSoup (2019–2025)
- Sentiment analysis using four models: FinBERT, DeBERTa, SVM, and Naive Bayes
- Technical indicator computation: EMA (14-day) and RSI (14-day)
- LSTM-based time-series forecasting for next-day price direction
- Hybrid signal generation: trade only when sentiment and LSTM agree
- Backtesting with full trading metrics: win rate, profit factor, net profit, drawdown
- Ground-truth sentiment validation with a professional Forex trader (12 years experience)

---

## System Architecture

The pipeline is modular and consists of two parallel streams that merge into a hybrid trading signal.

### Sentiment Stream

1. Raw news headlines are collected from FXStreet
2. Text is preprocessed: lowercasing, punctuation removal, stop word removal, tokenization
3. FinBERT and DeBERTa classify sentiment via transformer encoder layers using the [CLS] token output
4. SVM and Naive Bayes classify sentiment using TF-IDF vectorized features
5. Scores are mapped to +1 (positive), 0 (neutral), -1 (negative) and averaged daily per currency (EUR / USD)

### Price and Technical Stream

1. EUR/USD daily OHLCV data is collected from Yahoo Finance
2. EMA (14-day) and RSI (14-day) are computed from close prices
3. Missing values are filled using backfill to preserve time-series continuity
4. Features are normalized using StandardScaler

### Feature Fusion and LSTM Forecasting

1. Daily sentiment scores are merged with price and indicator features
2. A 5-day sliding window creates input sequences for the LSTM
3. The LSTM model (2 layers, 50 hidden units) predicts next-day price direction: +1 (up), 0 (no change), -1 (down)
4. Training: 10 epochs, CrossEntropyLoss, Adam optimizer (lr = 0.001), 80/20 train/test split

### Hybrid Signal Logic

A trade is executed only when the LSTM prediction and the sentiment model output agree on direction. This agreement filter reduces noise and improves signal quality at the cost of trade frequency.

---

## Technologies Used

| Category | Technologies |
|---|---|
| Programming | Python |
| Data collection | BeautifulSoup, Yahoo Finance API |
| Data processing | Pandas, NumPy, PySpark |
| NLP models | FinBERT, DeBERTa, SVM, Naive Bayes |
| ML framework | PyTorch (LSTM), Scikit-learn |
| Forecasting | LSTM (time-series), TF-IDF vectorization |
| Visualization | Matplotlib, Streamlit |

---

## Model Performance (Backtest: 2019–2025, Starting Balance: EUR 10,000)

| Model | Final Balance | Net Profit | Trades | Win Rate | Profit Factor |
|---|---|---|---|---|---|
| FinBERT | EUR 38,943.61 | EUR 28,943.61 | 361 | 59.00% | 2.28 |
| Naive Bayes | EUR 13,053.23 | EUR 3,053.23 | 267 | 47.94% | 1.22 |
| SVM | EUR 12,712.57 | EUR 2,712.57 | 127 | 51.18% | 1.53 |
| Hybrid FinBERT + LSTM | EUR 22,801.48 | EUR 12,801.48 | 186 | 63.98% | 2.91 |
| DeBERTa | EUR 9,994.41 | -EUR 5.59 | 1 | 0.00% | 0.00 |

**Recommended model:** Hybrid FinBERT + LSTM achieves the highest win rate (63.98%) and best profit factor (2.91), making it the most suitable strategy for risk-adjusted, consistent performance — particularly for prop firm funded account environments. FinBERT standalone produces the highest absolute profit for higher-frequency use cases.

---

## Sentiment Model Evaluation

Sentiment labels were validated against a ground-truth dataset hand-coded by a professional Forex trader with over 12 years of experience.

| Model | Accuracy | Precision | Recall | F1-Score |
|---|---|---|---|---|
| FinBERT | 0.858 | 0.8596 | 0.7829 | 0.8079 |
| Naive Bayes | 0.338 | 0.3294 | 0.3270 | 0.3114 |
| SVM | 0.318 | 0.3001 | 0.3066 | 0.2300 |
| DeBERTa | 0.188 | 0.2652 | 0.3532 | 0.1429 |

FinBERT significantly outperforms the other models due to its domain-specific financial pretraining. DeBERTa's poor performance despite its architectural sophistication confirms that domain pretraining matters more than model size for financial NLP tasks.

---

## Trading Strategy Logic

- Enter BUY when: positive sentiment signal + LSTM predicts upward movement
- Enter SELL when: negative sentiment signal + LSTM predicts downward movement
- Risk per trade: 1% of account balance
- Minimum risk-reward ratio: 1:2
- No trades executed during high-impact news windows (in line with prop firm policies)

---

## Project Structure

```
trading-llm-strategy/
│
├── data/                       # Raw and processed datasets
│   ├── eurusd_prices.csv
│   └── fxstreet_news.csv
│
├── models/                     # Trained model checkpoints
│
├── notebooks/                  # Jupyter notebooks for exploration
│
├── src/
│   ├── data_collection.py      # FXStreet scraping and Yahoo Finance ingestion
│   ├── preprocessing.py        # Text cleaning, TF-IDF, StandardScaler
│   ├── sentiment_analysis.py   # FinBERT, DeBERTa, SVM, NB inference
│   ├── model_training.py       # LSTM training and evaluation
│   ├── backtesting.py          # Trading simulation and metrics
│
├── app/                        # Streamlit dashboard
├── results/                    # Equity curves, confusion matrices, metrics
├── README.md
└── requirements.txt
```

---

## Installation and Setup

### Clone the repository

```bash
git clone https://github.com/soorajnikam/Trading-Strategies-with-LLM-and-Traditional-Model
cd Trading-Strategies-with-LLM-and-Traditional-Model
```

### Install dependencies

```bash
pip install -r requirements.txt
```

### Run model training

```bash
python src/model_training.py
```

### Launch the Streamlit dashboard

```bash
streamlit run app/app.py
```

---

## Real-World Considerations

**Prop firm compatibility:** The hybrid model operates within standard prop firm risk parameters — 1% risk per trade, minimum 1:2 risk-reward ratio, and avoidance of news window trading — making it suitable for funded account strategies at firms such as FTMO, Funded Next, and MyFunded FX.

**Slippage and execution delay:** In high-volatility periods, orders may not fill at the intended price. The strategy addresses this by using limit and stop entries and avoiding execution during news spikes.

**News trading restrictions:** Most prop firms prohibit live trading during high-impact news events. This system uses pre-event sentiment analysis via FinBERT so trading decisions are informed by news context without executing trades in restricted windows.

---

## Future Improvements

- Integration with real-time news APIs (Bloomberg Terminal, Reuters Eikon)
- Live trading deployment via MetaTrader 4/5 Expert Advisors or broker API webhooks
- Extended backtesting across 20 years of EUR/USD data including pre-2008 crisis regimes
- Reinforcement learning for adaptive position sizing
- Multi-asset extension to additional Forex pairs, crypto, and indices
- Integration of advanced LLMs (GPT-4, Gemini) for deeper contextual news reasoning

---

## References

Full bibliography available in the project report. Key references include Pornwattanavichai et al. (2022) BERTFOREX, Xing (2020) FinBERT Forex application, and Salbrechter (2023) FinNewsBERT.

---

## Appendix

All datasets, code, and results are available in this repository.

Word cloud analysis of EUR/USD news corpus highlights dominant terms: Dollar, ECB, Fed, EUR, rate — confirming the sentiment focus of the scraped dataset.
