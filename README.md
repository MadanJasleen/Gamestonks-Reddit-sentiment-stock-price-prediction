# Gamestonks-Reddit-sentiment-stock-price-prediction
Predicting GameStop (GME) stock prices with Reddit (r/WallStreetBets) sentiment signals, using LLM-based prompt sentiment analysis + multistage LSTM framework at 5-minute intervals.
Link to full paper : 

##Description
This project investigates the role of online investor sentiment in shaping intraday stock price movements during the GameStop (GME) saga. Using discussion threads from the r/WallStreetBets subreddit between December 15, 2020 and February 28, 2021, I apply a context-aware, prompt-based sentiment analysis framework powered by the Gemma 3 (12B parameter) large language model. Unlike traditional lexicon-based sentiment methods that often misclassify financial slang and meme-driven discourse, this approach captures the nuances of retail investor sentiment in the unique environment of Reddit.

I then integrate these sentiment signals with high-frequency stock price data in a non-autoregressive Long Short-Term Memory (LSTM) multistage framework, evaluating predictive performance at 5-minute intervals. Our results show that while price data alone establishes a reasonable baseline, augmenting it with Reddit activity metrics (comment counts, votes) and net sentiment (positive – negative) significantly improves predictive accuracy — with increases in R² and reductions in Mean Absolute Error (MAE).

Findings suggest that online community sentiment not only reflects but can also anticipate price movements, particularly during euphoric and bullish phases of the meme-stock phenomenon. This work contributes to growing research on meme stocks, retail investor dynamics, and the intersection of social media and financial markets, offering insights into how collective online behavior can influence short-term market volatility.

## Datasets

This study relies on two primary datasets:
1. **Reddit Data** — collected from the [r/wallstreetbets subreddit](https://www.reddit.com/r/wallstreetbets/).  (15 Dec 2020 to 15 Feb 2021)
   - The Reddit data was originally obtained via the **Pushshift API**, which provided access to archived Reddit data prior to its decommissioning in April 2023.  
   - The datasets are now hosted on [Academic Torrents](https://academictorrents.com/details/1614740ac8c94505e4ecb9d88be8bed7b6afddd4), curated by the Reddit user [Watchful1](https://www.reddit.com/user/Watchful1/).  

2. **Stock Price Data** — minute-level GameStop (GME) price information obtained from [FirstRate Data](https://firstratedata.com/).  


## Methodology

1. Input Design
   -12 consecutive five-minute GME price observations (1-hour history).
   -Non-autoregressive: only past data used, no prediction feedback.
   -One-hour prediction gap prevents trivial replication of recent prices and encourages learning broader trends.
2. Training Objective
   - Loss function: Mean Squared Error (MSE)
   - Encourages robust trajectory learning beyond short-term autocorrelations.
3. Modeling Stages
   - Baseline (Price-Only) : 1-hour past price context → predict price 1 hour ahead (with 1-hour gap)
   - With Reddit Activity (Engagement Proxies): Adds r/wallstreetbets activity (comment volume, net scores) during the gap. Reflects retail investor attention and discourse intensity.
   - With Reddit Sentiment (Semantic Integration): Replaces engagement proxies with sentiment scores (bullish / bearish / neutral). Extracted via large language models to capture narrative and emotional content.
4. Overall Strategy
   - Progression from price-only → activity-based → sentiment-rich features.
   - Designed to test whether retail investor sentiment on Reddit can predict GME price movements.
     

## Findings

Each stage experiment was repated 30 times on fixed set of hyperparameters for comparison and reliability -  
seq_length = 12
batch_size = 32
learning_rate = 0.0001
num_epochs = 600

The following table includes the Mean Square Error MSE during the squeeze and post the squeeze


| During Squeeze (20 Jan - 27 Jan) | After Squeeze (28 Jan - 04 Feb) |
|----------|----------|----------|
|Price only | 192.122 ± 36.020 | 343.397 ± 110.419|
|Price with Redit engagement proxies | 161.570 ± 25.610 | 254.253 ± 58.900|
|Price with Reddit sentiments | 134.755 ± 14.794| 195.691 ± 27.485|

#3 Implications

- Social dynamics drive markets: Findings support that coordinated retail sentiment, not just individual motives, shaped GME’s price action.

- Herding and cascades: Reddit activity fueled synchronized trading through herding behavior and information cascades, where viral posts triggered waves of retail participation.

- Limits of engagement metrics: Predictive power dropped post-squeeze as sentiment fragmented, showing that raw activity counts alone are insufficient in volatile phases.

- Power of sentiment signals: Sentiment-based features (Stage 3) provided stronger predictive signals, especially during the squeeze, underscoring the importance of emotional and narrative content in speculative markets.

