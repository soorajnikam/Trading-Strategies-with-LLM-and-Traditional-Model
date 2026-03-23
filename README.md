

# Forex Trading Strategy Using Financial NLP (FinBERT) and Predictive Machine Learning Models

##  Project Overview

This project presents a **data-driven trading strategy for the EUR/USD forex pair**, leveraging **LLM-based sentiment analysis** and **machine learning models** to generate trading signals and evaluate performance.

The system integrates **financial data, news sentiment, and predictive modeling** to enhance trading decision-making.

---

##  Key Features

* 1. Real-time data collection from Yahoo Finance
* 2. News sentiment analysis using **FinBERT** & **RoBERTa**
* 3. Machine learning models for prediction (SVM, Naive Bayes, LSTM)
* 4. Backtesting with trading metrics (Win Rate, Profit Factor, Net Profit)
* 5. Hybrid model combining sentiment + ML predictions
* 6. Visualization and reporting of trading performance

---

##  Architecture Overview

The system follows a modular pipeline:

1. **Data Collection**

   * Real-time EUR/USD price data (Yahoo Finance)
   * Financial news datasets

2. **Data Preprocessing**

   * Cleaning & normalization using PySpark & Pandas
   * Manual labeling of news data aligned with market movement

3. **Sentiment Analysis (NLP)**

   * FinBERT (Financial sentiment-specific model)
   * RoBERTa (General NLP enhancement)
   * Output: Sentiment scores (Positive / Negative / Neutral)

4. **Feature Engineering**

   * Combine price data + sentiment scores
   * Generate trading signals

5. **Model Training**

   * Machine Learning Models:

     * SVM
     * Naive Bayes
     * LSTM (for time-series forecasting)

6. **Hybrid Model**

   * Combines FinBERT sentiment + ML predictions
   * Improves accuracy and trading performance

7. **Evaluation & Backtesting**

   * Metrics:

     * Win Rate
     * Profit Factor
     * Net Profit
     * Total Trades

8. **Visualization**

   * Streamlit / Matplotlib dashboards

---

##  Technologies Used

| Category        | Technologies           |
| --------------- | ---------------------- |
| Programming     | Python                 |
| Data Processing | PySpark, Pandas        |
| NLP Models      | FinBERT, RoBERTa       |
| ML Models       | SVM, Naive Bayes, LSTM |
| Data Source     | Yahoo Finance          |
| Visualization   | Matplotlib, Streamlit  |

---

##  Model Performance

### 1. FinBERT

* Initial Balance: €10,000
* Final Balance: €38,943.61
* Net Profit: €28,943.61
* Trades: 361
* Win Rate: 59.00%
* Profit Factor: 2.28

### 2. Naive Bayes

* Final Balance: €13,053.23
* Net Profit: €3,053.23
* Win Rate: 47.94%
* Profit Factor: 1.22

### 3. SVM

* Final Balance: €12,712.57
* Net Profit: €2,712.57
* Win Rate: 51.18%
* Profit Factor: 1.53

### 4. Hybrid FinBERT Model ---> Proposed Model

* Final Balance: €22,801.48
* Net Profit: €12,801.48
* Win Rate: 63.98%
* Profit Factor: 2.91

---

##  Project Structure

```
trading-llm-strategy/
│
├── data/                  # Raw & processed datasets
├── models/                # Trained ML models
├── notebooks/             # Jupyter notebooks
├── src/
│   ├── data_collection.py
│   ├── preprocessing.py
│   ├── sentiment_analysis.py
│   ├── model_training.py
│   ├── backtesting.py
│
├── app/                   # Streamlit dashboard
├── results/               # Evaluation outputs
├── README.md
└── requirements.txt
```

---

##  Installation & Setup

### 1️ Clone Repository

```bash
git clone https://github.com/your-username/trading-llm-strategy.git
cd trading-llm-strategy
```

### 2️ Install Dependencies

```bash
pip install -r requirements.txt
```

### 3️ Run Project

```bash
python src/model_training.py
```

##  Trading Strategy Logic

* Use **sentiment score + price trend**
* Enter **BUY** when:

  * Positive sentiment + upward momentum
* Enter **SELL** when:

  * Negative sentiment + downward momentum
* Risk-Reward Ratio: **2:10**
* Low stop-loss strategy for high-frequency trades

---

##  Future Improvements

*  Integrate real-time news APIs (Bloomberg, Reuters)
*  Use advanced LLMs (GPT, Gemini) for deeper reasoning
*  Add reinforcement learning for adaptive trading
*  Deploy on AWS for real-time trading system

---

##  Conclusion

This project demonstrates how **LLM-based sentiment analysis combined with machine learning** can significantly improve trading strategies in forex markets.

The **Hybrid FinBERT model** achieved the best performance, highlighting the importance of combining **NLP + quantitative models**.


