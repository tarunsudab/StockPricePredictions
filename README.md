**AAPL Weekly Direction Forecasting with Machine Learning & NLP**

**Overview**

This project develops a probability-based machine learning model to predict whether Apple (AAPL) stock will increase over the next 5 trading days (weekly horizon).

Instead of forecasting exact prices, the model outputs the probability of a positive return, which is converted into a trading signal using a calibrated confidence threshold. The strategy is evaluated using a realistic, non-overlapping weekly backtest to assess economic impact rather than relying solely on classification accuracy.

**Disclaimer**

This project is for educational purposes only and does not constitute financial advice.

**Objective**

- Predict AAPL’s weekly price direction (up/down)
- Integrate structured market data with financial news sentiment
- Convert model probabilities into confidence-based decisions
- Evaluate economic usefulness through backtesting

**Data Sources**

**Market Data**
- Daily close prices for:
  - AAPL
  - Market proxies: SPY, QQQ, XLK
  - Bond proxies: TLT, IEF

**News Data**
- Financial headlines from the Finnhub API (1-year free tier)

**Sentiment Model**
- FinBERT (financial-domain BERT model via Hugging Face Transformers)

**Feature Engineering**

**Market Features**
- Daily percentage returns for AAPL and market proxies
- Cross-asset context to capture broader market regimes

**NLP Features (aggregated daily)**
- Headline count
- Mean positive, negative, neutral sentiment
- Aggregated sentiment intensity
- Binary indicator for presence of news

All features are strictly backward-looking to prevent data leakage.

**Target Definition**

Weekly classification target:

$$y_t = \mathbb{1}(\text{Close}_{t+5} > \text{Close}_t)$$

- 1 → AAPL higher in 5 trading days
- 0 → AAPL not higher in 5 trading days

**Modeling Approach**
- Model: Random Forest Classifier
- Validation: Time-based train / validation / test split
- Output: Probability of positive 5-day return
- Decision Rule: $$\text{Trade if } P(\text{UP in 5 days}) \geq 0.55$$

The threshold was selected using a precision–recall tradeoff analysis to prioritize decision quality over trade frequency.

**Model Performance (Weekly Horizon)**
- Directional Accuracy: ~59–61%
- Precision (UP class after thresholding): ~63–65%
- Recall (UP class): ~55–60%
- High-confidence predictions reaching ~75% probability

The focus is on confidence-calibrated decision-making, not raw accuracy alone.

**Backtesting Methodology**

A realistic weekly backtest was implemented:
- Long-only strategy
- 5-trading-day holding period
- Non-overlapping returns (to prevent artificial compounding)
- Stay in cash during low-confidence periods
- Compared against buy-and-hold

**Key Results**
- The ML strategy slightly outperforms buy-and-hold in the test period
- Exposure is reduced during lower-confidence regimes
- Equity curve behavior is realistic and stable
- Drawdowns are generally smoother than buy-and-hold
- The strategy emphasizes risk-aware participation, not constant market exposure.

**Example Signal**
Example high-confidence weekly prediction:
- Predicted probability of upward move: ~0.75
- Expected 5-day return (historical conditional estimate): ~+1.5%
- Illustrative projected price move: ~$278 → ~$283

This reflects probabilistic expectation, not a guaranteed outcome.

**Tech Stack**
- Python
- Pandas / NumPy
- Scikit-learn
- Hugging Face Transformers (FinBERT)
- Matplotlib

**Limitations**
- No transaction costs or slippage modeled
- News data limited to 1-year history (API constraint)
- Single-asset focus (AAPL)

**Future Improvements**
- Add transaction costs and realistic execution assumptions
- Expand to multi-asset portfolio
- Confidence-based position sizing
- Compare against gradient boosting models (XGBoost, LightGBM)
- Extend NLP history beyond free-tier limits
