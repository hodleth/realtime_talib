# Realtime TA-Lib

Fast & Lightweight library that calculates TA-Lib Technical Indicators live. **Built with Python 2.7!**

**Still a WIP,** but my first real Python project!

The Quantopian and Pipeline API's are confusing at first, backtrader is confusing... the list goes on.
It's pretty easy to get overwhelmed.

Personally, I think this could be a really useful tool for a beginner in algotrading. 

Experienced back-testers might also find this useful, mainly because of the speed and abstraction that
Realtime TA-Lib provides.

**Basic Overview:**
* Uses only native Python libraries (urllib, json) to scrape the Google/Yahoo Finance JSON data
* Parses JSON and formats historical & live data into 2D/1D Lists
* Converts Lists to NumPy Arrays to form TA-Lib input dictionary
* [TA-Lib Abstract API](https://mrjbq7.github.io/ta-lib/abstract.html) does remainder of the grunt work

## Installation
```
pip install realtime_talib
```

## Documentation

### Example:

```python
import realtime_talib as rtt
from rtt import realtime, pipeline, indicators
import time

init = realtime_init('NASDAQ','SPY','2016-01-01','2017-02-01',False)

while True:
	
	print MA(1, 3, init)
	print MACD(1, 12, 26, 9, init)[2]
	print BBANDS(1, 2, 2, 10, init)[1]
	print RSI(1, 25, init)

	time.sleep(5)
	
...

 Terminal Outputs:
 226.92278436
 0.0184101640085
 227.320467648
 62.1289093161
```

### Basic Indicator Functions:

* `realtime_init(exchange, ticker, start_date, end_date, inc_delayed_volume)`
* `MA(ma_type, timeperiod, init)`
* `MACD(ma_type, fastperiod, slowperiod, signalperiod, init)`
* `RSI(ma_type, timeperiod, init)`
* `BBANDS(ma_type, nbdevup, nbdevdn, timeperiod, init)`

(Check the docs at the [RTT Temporary Indicator Doc](https://shrib.com/9G1SclqXIIwm2Ep) for the full list.)

### Other Useful Functions (Raw Data):

* `pull_live_data(exch, ticker)`
* `pull_historical_data(ticker, start_date, end_date)`
* `formatted_price(exch, ticker, refresh_rate)`

pull_live_data returns a 1D List, and pull_historical_data returns a 2D List.
* `pull_live_data` format: `[ticker_data]`(0 = Current Time|1 = Current Price|2 = 15-Min Delayed Vol)
* `pull_historical_data` format: `[day][ticker_data]`(0 = Date|1 = Open|2 = Close|3 = High|4 = Low|5 = Vol)

(Functions support NASDAQ and NYSE, and all the tickers under them. Refresh rate is in seconds.)

```python
import realtime_talib as rtt
from rtt import pipeline 

# Inputs:
print pull_live_data('NYSE','SPY')[1]
print pull_historical_data('NVDA','2017-01-31','2017-02-03')[1][2] #January 31st, 2017
print formatted_price ('NASDAQ','AMD',5)

...

Terminal Outputs:
229.34
108.39
AMD| Price: 12.24 | Change: -0.33%
```

## TODO

- [ ] PyPI support (pip install...)
- [x] Multi-threading support
- [x] Parsing Yahoo Finance historical data
- [x] Calculating Indicators on the fly
- [ ] Add support for 40+ TA-Lib indicators
- [ ] Matplotlib & Pyfolio integration (possibly)
