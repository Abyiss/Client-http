# Client-http

LAST UPDATED: 12/21/2021

***At the current time, while Abyiss is under further development, this is the ONLY Abyiss client documentation that is in working order.***

This documentation encompasses the raw HTTP Endpoints for the Abyiss API.
All Endpoints should return some raw json data (except for the page redirects).

While the API is in beta, we will offer it as a free service and will not be charging any money. When we transition out of beta, we will offer it as a paid service. We suggest signing up for a free account to take advantage of our API and additional offers [Sign Up](https://www.abyiss.com/login).

If you have any problems with this library, please open an issue request on [Github](https://github.com/Abyiss/Client-http/issues). To find more information about Abyiss please visit [Abyiss.com](https://www.abyiss.com/). For any additional support please email us at [contact@abyiss.com](mailto:contact@abyiss.com).


# Getting Started

Our base URL endpoint: 
NOTE: This base url with no appened routes/endpoints will redirect to our home page

### [http://api.abyiss.com/](http://api.abyiss.com/)

To learn more about our other endpoints check out our website [docs.abyiss.com/](https://docs.abyiss.com/) or continue reading below:

## Endpoints and Routes

![Endpoint](https://img.shields.io/badge/-get-green) ***/ping***
* Example URL: http://api.abyiss.com/ping
* Returns a 200 status code upon successful query.
* Returns a static object:
  - **ping**: "Hello Abyiss"
* Example Response:
    ```json
    {"ping":"Hello from the Abyiss"}
    ```

### Cryptocurrency Exchanges

![Endpoint](https://img.shields.io/badge/-get-green) ***/v1/exchanges***
* Returns a 200 status code upon successful query.
* Returns an array of all exchanges in the form of market objects that the api offers.
* Response market object properties:
  - **name**: String. The official name of the exchange.
  -  **id**: String. The id of the exchange used within the api routes.
* Example Response:
    ```
    /v1/exchanges
    ```
    ```json
    [
        {
            "name":"Binance",
            "id":"binance"
        },
        {
            "name":"Binance US",
            "id":"binanceus"
        },
        {
            "name":"Coinbase Pro",
            "id":"coinbasepro"
        },
        {
            "name":"BitBay",
            "id":"bitbay"
        }
    ]
    ```

![Endpoint](https://img.shields.io/badge/-get-green) ***/v1/{exchange id}***
* Returns a 200 status code upon successfuly query.
* Returns an objects with properties about the exchange.
* Response object properties:
  - **name**: String. the official name of the exchange.
  - **id**: String. the id of the exchange used within the api routes.
  - **url**: String. the exchange's official website url.
  - **hasTrades**: Boolean. Whether the api can be used to query market trades on the exchange.
  - **hasAggregates**: Boolean. Whether the api can be used to query market candle aggregates on the exchange.
  - **aggregateTimeframes**: Object containing all of the timeframes supported for market candle aggregates.
* Example Response:
    ```
    /v1/coinbasepro
    ```
    ```json
    {
        "name":"Coinbase Pro",
        "id":"coinbasepro",
        "url":"https://pro.coinbase.com/",
        "hasTrades":true,
        "hasAggregates":true,
        "aggregateTimeframes":
        {
            "1m":60,
            "5m":300,
            "15m":900,
            "1h":3600,
            "6h":21600,
            "1d":86400
        }
    }
    ```

![Endpoint](https://img.shields.io/badge/-get-green) ***/v1/{exchange id}/status***
* Returns a 200 status code upon successful query.
* Returns an object with properties that describe an exchange's status.
* Response object properties:
  - **status**: String. The status of the exchange. 'ok' is good.
  - **updated**: Int. Unix timestamp of last time the exchage's status was updated.
* Example Response:
    ```
    /v1/coinbasepro/status
    ```
    ```json
    {
        "status":"ok",
        "updated":1634929487916
    }
    ```

![Endpoint](https://img.shields.io/badge/-get-green) ***/v1/{exchange id}/markets***
* Returns a 200 status code upon successful query.
* Returns a list of all market ids on the exchange.
* Example Response:
    ```
    /v1/coinbasepro/markets
    ```
    ```json
    [
        "OGN/BTC",
        "REQ/BTC",
        "KEEP/USD",
        "AAVE/USD",
        "SKL/GBP",
        "MIR/EUR",
        "FORTH/EUR",
        "DOT/USDT"
    ]
    ```

![Endpoint](https://img.shields.io/badge/-get-green) ***/v1/{exchange id}/{market id}***
* Returns a 200 status code upon successful query.
* Returns an object with properties about the market.
* Response object properties:
  - **exchange**: String. The exchange id the market is on.
  - **symbol**: String. The symbol of the market.
  - **active**: Boolean. Whether the market is active on the exchange.
  - **base**: String. The base of the market. eg: The quantity that is bought.
  - **quote**: String. The quote of the market. eg: The currency being compared.
  - **percentage**: Boolean. 
  - **taker**: Float. 
  - **maker**: Float.
  - **spot**: String. 
* Example Response:
    ```
    /v1/coinbasepro/BTC-USD
    ```
    ```json
    {
        "exchange":"coinbasepro",
        "symbol":"BTC/USD",
        "id":"BTC-USD",
        "active":true,
        "base":"BTC",
        "quote":"USD",
        "percentage":true,
        "taker":0.005,
        "maker":0.005,
        "type":"spot"
    }
    ```

![Endpoint](https://img.shields.io/badge/-get-green) ***/v1/{exchange id}/{market id}/trades***
* Returns a 200 status code upon successful query.
* Returns an array of recent trades that have occurred on an exchange for that market.
* Recent trade properties:
  - **exchange**: String. The exchange id the trade occurred on.
  - **market**: String. The market id - what was traded.
  - **id**: String. The -exchange specific- unique id of the trade. Some exchanges do not have this.
  - **timestamp**: String. The unix timestamp that trade was executed.
  - **price**: Float. The quote currency price of the market.
  - **size**: Float. The quantity traded.
  - **cost**: Float. The quote cost: size * price.
  - **side**: String. Whether the trade was a buy or sell.
* Example Response:
    ```
    /v1/coinbasepro/BTC-USD/trades
    ```
    ```json
    [
        {
            "exchange":"coinbasepro",
            "market":"BTC/USD",
            "id":"225294919",
            "timestamp":"1634929741281",
            "price":60344.64,
            "size":0.00013204,
            "cost":7.967906265599999,
            "side":"buy"
        },
        {
            "exchange":"coinbasepro",
            "market":"BTC/USD",
            "id":"225294920",
            "timestamp":"1634929741466",
            "price":60344.64,
            "size":0.00004882,
            "cost":2.9460253248,
            "side":"buy"
        },
        {
            "exchange":"coinbasepro",
            "market":"BTC/USD",
            "id":"225294922",
            "timestamp":"1634929741579",
            "price":60347.03,
            "size":0.00994284,
            "cost":600.0208637651999,
            "side":"buy"
        },
        {
            "exchange":"coinbasepro",
            "market":"BTC/USD",
            "id":"225294921",
            "timestamp":"1634929741579",
            "price":60344.64,
            "size":0.00150799,
            "cost":90.99911367360001,
            "side":"buy"
        }
    ]
    ```

![Endpoint](https://img.shields.io/badge/-get-green) ***/v1/{exchange id}/{market id}/aggregates/{aggregate size}***
* Returns a 200 status code upon successful query.
* Returns an array of recent aggregate candlesticks for a given aggregate size on a given market on a given exchange.
* aggregate size is exchange-specific and are listed in the endpoint: /v1/{exchange}
* Recent aggregate properties:
  - **exchange**: String. The exchange id the aggregate was calculated on.
  - **market**: String. The market id the aggregate was calculated on.
  - **timestamp**: Int. The unix timestamp for the start of the aggregate calculation, which defines the aggregate's scope.
  - **open**: Float. The first, or opening, price of the aggregate's scope.
  - **high**: Float. The highest price recorded within the scope of the aggregate.
  - **low**: Float. The lowest price recorded within the scope of the aggregate.
  - **close**: Float. The last, or closing, price within the aggregate's scope.
  - **volume**: Float. The volume within the aggregate's scope.
* Example Response:
    ```
    /v1/coinbasepro/btc-usd/aggregates/1m?limit=3
    ```
    ```json
    [
        {
            "exchange":"coinbasepro",
            "market":"BTC/USD",
            "timestamp":1635171720000,
            "open":63217.48,
            "high":63224.51,
            "low":63167.99,
            "close":63182.8,
            "volume":7.6732551
        },
        {
            "exchange":"coinbasepro",
            "market":"BTC/USD",
            "timestamp":1635171780000,
            "open":63182.79,
            "high":63207.06,
            "low":63157.97,
            "close":63181.38,
            "volume":6.05976851
        },
        {
            "exchange":"coinbasepro",
            "market":"BTC/USD",
            "timestamp":1635171840000,
            "open":63176.86,
            "high":63189.6,
            "low":63157.97,
            "close":63161.38,
            "volume":3.70058086
        }
    ]
    ```

![Endpoint](https://img.shields.io/badge/-get-green) ***/v1/{exchange}/{market}/orders***
* Returns a 200 status code upon successful query
* Returns a snapshop of the recent order book on an exchange
* Data returned is in the form of an array of bids and an array of asks
* Each exchange returns slightly different schemas, so some exchanges may not return all the properties.
* Order book properties:
  - **exchange**: String. The exchange id the order book is from.
  - **market**: String. The market id the order book is about.
  - **timestamp**: Int. Unix timestamp of the order book
  - **datetime**: String. The datetime timestamp of the order book.
  - **nonce**: Int. The serial unique identifier of the order book.
  - **bids**: Array of arrays of bids in the market [[float, float], [float, float]]
  - **asks**: Array of arrays of asks in the market [[float, float], [float, float]]
* Example Response:
    ```
    /v1/binance/btc-usdt/orders
    ```
    ```json
    {
        "exchange":"binance",
        "market":"BTC/USDT",
        "nonce":14601360013,
        "bids":
        [
            [
                61947.91,
                1.48088
            ],
            [
                61947.9,
                0.5
            ],
            [
                61944.07,
                0.44094
            ]
        ],
        "asks":
        [
            [
                61947.92,
                2.28573
            ],
            [
                61952.89,
                0.11214
            ],
            [
                61952.9,
                0.50224
            ]
        ]
    }
    ```

### Redirects

![Endpoint](https://img.shields.io/badge/-get-green) ***/*** 
- redirects to https://abyiss.com

![Endpoint](https://img.shields.io/badge/-get-green) ***/abyiss*** 
- redirects to https://abyiss.com
