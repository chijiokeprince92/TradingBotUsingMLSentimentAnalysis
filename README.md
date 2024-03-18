# Trading Bot with Alpaca API and Sentiment Analysis

## Overview

This repository contains a trading bot that leverages the Alpaca API for executing trades and incorporates a machine learning sentiment analysis model to make trading decisions based on news sentiment. The bot aims to automate trading strategies by buying or selling assets on the Alpaca platform based on the sentiment of financial news.


This code is an example of using the LumiBot library for algorithmic trading on the Alpaca trading platform. It implements a machine learning-based trading strategy using sentiment analysis on financial news headlines, with the help of the FinBERT model for sentiment estimation.

Here's a breakdown of what's happening:

1. The required modules and libraries are imported, including the Alpaca broker, LumiBot components, and the sentiment estimation utility from the previous example.

2. The Alpaca API credentials are loaded from the environment variables using the `dotenv` library.

3. The `MLTrader` class is defined, which inherits from the `Strategy` class provided by LumiBot.

4. In the `initialize` method, the trading symbol, cash-at-risk percentage, and Alpaca API instance are set up.

5. The `position_sizing` method calculates the available cash, the last price of the trading symbol, and the quantity of shares to trade based on the cash-at-risk percentage.

6. The `get_dates` method retrieves the current date and the date three days prior, which will be used to fetch news headlines.

7. The `get_sentiment` method fetches news headlines for the trading symbol from the Alpaca API, passes them to the `estimate_sentiment` function, and returns the sentiment probability and label.

8. The `on_trading_iteration` method is the main trading logic. It performs the following steps:
   a. Calculates the available cash, last price, and quantity of shares.
   b. Gets the sentiment probability and label using the `get_sentiment` method.
   c. Checks if there is enough cash to trade and if the sentiment probability is high enough (above 0.999).
   d. If the sentiment is positive and the probability is high, it creates a buy order with a take-profit and stop-loss order.
   e. If the sentiment is negative and the probability is high, it creates a sell order with a take-profit and stop-loss order.
   f. Submits the order and updates the `last_trade` status.

9. The backtest is set up with the `MLTrader` strategy, the Alpaca broker, and the specified start and end dates (2020-01-01 to 2023-12-31).

10. The backtest is run using the `YahooDataBacktesting` data source and the specified parameters.

11. The commented-out code at the end suggests how to create a `Trader` instance, add the strategy, and run it in live trading mode.

This code demonstrates how to integrate machine learning models (like FinBERT for sentiment analysis) into an algorithmic trading strategy using the LumiBot library and the Alpaca trading platform. It showcases the process of fetching news data, estimating sentiment, and executing trades based on the sentiment analysis results.

### Sentiment Analysis Section (fnbert_utils.py)

This code is using the Hugging Face Transformers library to load a pre-trained FinBERT model (a variant of BERT trained on financial data) and use it for sentiment analysis on financial news text.

Here's a breakdown of what's happening:

1. The necessary modules from the transformers library (`AutoTokenizer` and `AutoModelForSequenceClassification`) and PyTorch are imported.

2. The code checks if a CUDA-enabled GPU is available and assigns the appropriate device (`"cuda:0"` or `"cpu"`).

3. The `AutoTokenizer` and `AutoModelForSequenceClassification` are loaded from the pre-trained "ProsusAI/finbert" model.

4. A list of labels for the sentiment classes is defined: `["positive", "negative", "neutral"]`.

5. The `estimate_sentiment` function is defined. It takes a news text as input and performs the following steps:
   a. If the input text is not empty, it tokenizes the text using the loaded tokenizer and sends the input to the specified device.
   b. The tokenized input is passed through the loaded model, and the logits (raw output values) are obtained.
   c. The logits are passed through a softmax function to obtain the probabilities for each class.
   d. The class with the highest probability is determined, and the corresponding probability value and sentiment label are returned.
   e. If the input text is empty, it returns a probability of 0 and the "neutral" label.

6. In the `if __name__ == "__main__":` block, the `estimate_sentiment` function is called with a list of two news texts, and the resulting probabilities and sentiment labels are printed.

7. Finally, it prints whether a CUDA-enabled GPU is available or not using `torch.cuda.is_available()`.

This code demonstrates how to use a pre-trained transformer model for sequence classification tasks, in this case, sentiment analysis on financial news text. The FinBERT model has been fine-tuned on financial data, which should improve its performance on domain-specific tasks compared to a general-purpose model like BERT.

### 3. Running the Bot

- Execute the main bot script: `python trading_bot.py`
- The bot will fetch real-time market data, perform sentiment analysis on news, and execute trades accordingly.



## Disclaimer

Trading involves risks, and the use of this bot does not guarantee profits. The bot's performance is based on historical data and sentiment analysis, which may not accurately predict future market conditions. Use this bot responsibly and consider consulting financial experts before making any financial decisions.

## Contributing

Some part of the code for building this bot was gotten from https://github.com/nicknochnack/MLTradingBot

Feel free to contribute to the development of this trading bot by submitting issues, feature requests, or pull requests.

## License

This trading bot is licensed under the [MIT License](LICENSE).
