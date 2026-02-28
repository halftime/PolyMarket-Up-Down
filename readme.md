# Polymarket Up/Down binary crypto markets
## Intro
Binary up/down markets exist in a wide variety @ https://polymarket.com/predictions/up-or-down
For sake of the write up I'll limit to the crypto markets

Timeframes for *live* trading:
- 5 min
- 15 min
- 1 hour
- 1 day (24h)

Crypto pairs:
- BTC (by far the most volume)
- ETH
- SOL

### Strike
By market start (T_0) a timer will start a countdown. (T_0) will be the reference "price to beat" = strike price.

Somewhat later (T_0 + x) (+- 30 s) the site UI will display the price to beat. If you want to be sure of your strike you will have to scrape this value (e.g: threaded worker > webdriver > regex > ...). 

You can approximate the strike earlier by logging the very last previous closing price (chainlink RTDS) or try a GET request it from API: https://polymarket.com/api/crypto/crypto-price?symbol=BTC&eventStartTime=2026-02-02T19:45:00Z&variant=fifteen 

Mind all GET params: symbol ; eventStartTime (ts as long format string) ; variant = fifteen | five | ?

... Combine methods for a fast approximation followed up by a accurate (scraped) strike

Market will stop trading when the timer finishes. It will resolve automatically **based on the chainlink price & strike**

### RTDS (real time data stream)
See: https://docs.polymarket.com/market-data/websocket/rtds

2 data stream exist which you can tap into using a websocket:
- by chainlink (remember this is what is used for auto-resolving markets) 
- by Binance (leading prices)

Assume Binance to be leading the chainlink oracle which has some delay to aggregate data, update the blockchain, etc.

Beware polymarket WS are known for stability issues (aggrevated by VPN usage). So feel free to source your price feeds elsewhere.

