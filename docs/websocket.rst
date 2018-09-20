WebSocket
=========

Connection
----------

Connect to spiral exchange websocket with wss://ws.spiral.exchange

Response
^^^^^^^^

.. code-block:: json
   :caption: connected message sent from server after connection established.

    {
        "event": "connected",
        "data": "welcome :)"
    }

Heartbeats
----------

WebSocket connection will be dropped if no data exchanged after a period of time (may be 1 minute).
To keep WebSocket connection, client can send heartbeats to server to reset connection timer.

Request
^^^^^^^
.. csv-table:: Request Data
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "**MUST** be ``ping``"
    "data", "Integer", "Current UNIX timestamp in seconds"

.. code-block:: json
   :caption: Ping to server

    {
        "event": "ping",
        "data": 1521182920
    }

Response
^^^^^^^^
.. csv-table:: Response Data
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "Server response event ``pong``"
    "data", "Integer", "Current UNIX timestamp in seconds"

.. code-block:: json
   :caption: Pong from server

    {
        "event": "pong",
        "data": 1521182920
    }


Authentication
--------------

Client must send ``authenticate`` event to subscribe private data, such as order/trade update.

Request
^^^^^^^
.. csv-table:: Request Data
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "**MUST** be ``authenticate``"
    "data.api_key", "String", "API access key"
    "data.expires", "Integer", "A UNIX timestamp after which the request is no longer valid. This is to prevent replay attacks."
    "data.signature", "String", "A signature of the request you are making. See :ref:`auth-signature-calc`"

.. code-block:: json
   :caption: Request

    {
        "event": "authenticate",
        "data": {
            "api_key": "abc0de9fg8hi7jkl6mnop5qrst4uvw3x2y1z",
            "expires": 1521182920,
            "signature": "zyx1w2v3ut4sr5q6pon7ml8kji9hg0fedcba"
        }
    }

Response
^^^^^^^^
.. csv-table:: Response Data
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "**MUST** be ``authenticated``"
    "data.success", "Boolean", "``True`` if succssed, otherwise a error response instead"

.. code-block:: json
   :caption: Response

    {
        "event": "authenticated",
        "data": {
            "success": true
        }
    }

Public Subscriptions
--------------------

.. Tip::
    Public subscriptions **DO NOT** need authentication.

Ticker
^^^^^^

Request
"""""""

.. csv-table:: Request Data
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "**MUST** be ``subscribe``"
    "data.topic", "String", "**MUST** be ``ticker``"
    "data.symbols", "String Array", "Symbol names of tickers to subscribe"

.. code-block:: json
   :caption: Request

    {
        "event": "subscribe",
        "data": {
            "topic": "ticker",
            "symbols": ["BTCUSDT", "ETHBTC", "ETHUSDT", "BCHUSDT", "BCHBTC", "LTCUSDT", "LTCBTC"]
        }
    }

Response
""""""""
.. csv-table:: Response Data
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "**MUST** be ``subscribed``"
    "data.topic", "String", "**MUST** be ``ticker``"
    "data.symbols", "String Array", "Symbol names of tickers to subscribe"

.. code-block:: json
   :caption: Response

    {
        "event": "subscribed",
        "data": {
            "topic": "ticker",
            "symbols": ["BTCUSDT", "ETHBTC", "ETHUSDT", "BCHUSDT", "BCHBTC", "LTCUSDT", "LTCBTC"]
        }
    }


Stream Data Segment
"""""""""""""""""""

.. csv-table:: Stream Data Segment
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "**MUST** be ``ticker``"
    "data.type", "String", "Currently, only ``24hrTicker`` (24 hours ticker) available"
    "data.symbol", "String", "Indicate symbol of current stream data segment"
    "data.open", "String", "Open price"
    "data.close", "String", "Close price"
    "data.high", "String", "High price"
    "data.low", "String", "Low price"
    "data.base_volume", "String", "Trade volume, take BTCUSDT for example, the unit is BTC"
    "data.quote_volume", "String", "Quote volume, take BTCUSDT for example, the unit is USDT"
    "data.timestamps", "Integer", "UNIX timestampe of stream data segment send, in millisecond"

.. code-block:: json
   :caption: Stream Data Segment

    {
        "event": "ticker",
        "data": {
            "type": "24hrTicker",
            "symbol": "BTCUSDT",
            "open": "5202.66",
            "close": "5345.17",
            "high": "5345.17",
            "low": "5345.17",
            "base_volume": "0.448",
            "quote_volume": "2394.63616",
            "timestamp": 1537324110000
        }
    }

Order Book
^^^^^^^^^^

Request
"""""""

.. csv-table:: Request Data
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "**MUST** be ``subscribe``"
    "data.topic", "String", "**MUST** be ``orderbook``"
    "data.symbols", "String Array", "Symbol names of orderbook to subscribe"

.. code-block:: json
   :caption: Request

    {
        "event": "subscribe",
        "data": {
            "topic": "orderbook",
            "symbols": ["ETHUSDT"]
        }
    }

Response
""""""""
.. csv-table:: Response Data
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "**MUST** be ``subscribed``"
    "data.topic", "String", "**MUST** be ``orderbook``"
    "data.symbols", "String Array", "Symbol names of orderbook to subscribe"

.. code-block:: json
   :caption: Response

    {
        "event": "subscribed",
        "data": {
            "topic": "orderbook",
            "symbols": ["ETHUSDT"]
        }
    }


Stream Data Segment
"""""""""""""""""""

.. csv-table:: Stream Data Segment
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "**MUST** be ``orderbook``"
    "data.symbol", "String", "Indicate symbol of current stream data segment"
    "data.last_update_id", "Integer", "Sequence number, order book may be sent in misorder. Client can use this id to sort order book data"
    "data.timestamps", "String", "UNIX timestampe of stream data segment send, in millisecond"
    "data.type", "Integer", "| 1: Incremental order book update
    | 2: Full snapshot order book
    | 3: 50 levels snapshot order book
    | 4: 100 levels snapshot order book"
    "data.data", "Array of Array", "Array of order book data"
    "data.data[][0]", "String", "Price"
    "data.data[][1]", "String", "Size"
    "data.data[][2]", "String", "Side. Refer to :doc:`Side <./dict>`"

.. code-block:: json
   :caption: Stream Data Segment

    {
        "event": "orderbook",
        "data": {
            "symbol": "ETHUSDT",
            "last_update_id": 2149622,
            "timestamp": 1537326202210237,
            "type": 1,
            "data": [["213.95", "0.574000", "bid"], ["213.93", "0.000000", "ask"]]
        }
    }


Trade
^^^^^

Request
"""""""

.. csv-table:: Request Data
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "**MUST** be ``subscribe``"
    "data.topic", "String", "**MUST** be ``trade``"
    "data.symbols", "String Array", "Symbol names of trades to subscribe"

.. code-block:: json
   :caption: Request

    {
        "event": "subscribe",
        "data": {
            "topic": "trade",
            "symbols": ["ETHUSDT"]
        }
    }

Response
""""""""
.. csv-table:: Response Data
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "**MUST** be ``subscribed``"
    "data.topic", "String", "**MUST** be ``trade``"
    "data.symbols", "String Array", "Symbol names of trades to subscribe"

.. code-block:: json
   :caption: Response

    {
        "event": "subscribed",
        "data": {
            "topic": "trade",
            "symbols": ["ETHUSDT"]
        }
    }


Stream Data Segment
"""""""""""""""""""

.. csv-table:: Stream Data Segment
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "**MUST** be ``trade``"
    "data.id", "Integer", "Trade id"
    "data.side", "String", "Refer to :doc:`Side <./dict>`"
    "data.symbol", "String", "Indicate symbol of current stream data segment"
    "data.price", "String", "Trade price"
    "data.quantity", "String", "Trade quantity"
    "data.timestamps", "String", "UNIX timestampe of stream data segment send, in millisecond"

.. code-block:: json
   :caption: Stream Data Segment

    {
        "event": "trade",
        "data": {
            "id": 674413,
            "side": "bid",
            "symbol": "ETHUSDT",
            "price": "211.41",
            "quantity": "0.007623",
            "timestamp": 1537339131911584
        }
    }

K-Line/Candles
^^^^^^^^^^^^^^

Request
"""""""

.. csv-table:: Request Data
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "**MUST** be ``subscribe``"
    "data.topic", "String", "**MUST** be ``kline``"
    "data.symbols", "String Array", "Symbol names of kline to subscribe"
    "data.period_minutes", Integer, "Period of minutes, minimum is 1"

.. code-block:: json
   :caption: Request

    {
        "event": "subscribe",
        "data": {
            "topic": "kline",
            "symbols": ["ETHUSDT"],
            "period_minutes": 1
        }
    }

Response
""""""""
.. csv-table:: Response Data
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "**MUST** be ``subscribed``"
    "data.topic", "String", "**MUST** be ``kline``"
    "data.period_minutes", Integer, "Period of minutes"
    "data.symbols", "String Array", "Symbol names of kline to subscribe"

.. code-block:: json
   :caption: Response

    {
        "event": "subscribed",
        "data": {
            "topic": "kline",
            "period_minutes": 1,
            "symbols": ["ETHUSDT"]
        }
    }


Stream Data Segment
"""""""""""""""""""

.. csv-table:: Stream Data Segment
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    :escape: \
    
    "event", "String", "**MUST** be ``kline``"
    "data", "Array", "Kline datas"
    "data.[0]", "Integer", "Open time, UNIX timestamp, in milliseconds"
    "data.[1]", "String", "Open price"
    "data.[2]", "String", "High price"
    "data.[3]", "String", "Low price"
    "data.[4]", "String", "Close price"
    "data.[5]", "String", "Traded volumn"
    "data.[6]", "Integer", "Close time, UNIX timestamp, in millisecond"
    "data.[7]", "String", "Reserved future usage, always \"0\""
    "data.[8]", "Integer", "Number of trades"

.. code-block:: json
   :caption: Stream Data Segment

    {
        "event": "kline",
        "data": [1537339020000, "211.46", "211.46", "211.2", "211.2", "0.071239", 1537339079000, "0", 3]
    }

Private Subscriptions
---------------------

.. Note::
    Private subscriptions **REQUIRED** Authentication

Order
^^^^^

Request
"""""""

.. csv-table:: Request Data
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "**MUST** be ``subscribe``"
    "data.topic", "String", "**MUST** be ``order``"
    "data.all_symbols", "Boolean", "``true`` if to subscribe all symbols orders. If true, ignore ``data.symbols`` field"
    "data.symbols", "String Array", "List of symbols of those orders to subscribe"

.. code-block:: json
   :caption: Request All Symbol Orders

    {
        "event": "subscribe",
        "data": {
            "topic": "order",
            "all_symbols": true
        }
    }

.. code-block:: json
   :caption: Request Certain Symbol Orders

    {
        "event": "subscribe",
        "data": {
            "topic": "order",
            "symbols": ["ETHBTC","BCHUSDT"]
        }
    }

Response
""""""""

.. csv-table:: Response Data
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80

    "event", "String", "**MUST** be ``subscribed``"
    "data.topic", "String", "**MUST** be ``order``"
    "data.all_symbols", "Boolean", "``true`` if to subscribe all symbols orders."
    "data.symbols", "String Array", "List of symbols of those orders subscribed"

.. code-block:: json
   :caption: Response Data

    {
        "event": "subscribed",
        "data": {
            "topic": "order",
            "all_symbols": true,
            "symbols": ["ETHBTC", "BCHUSDT", "BCHBTC", "LTCUSDT", "LTCBTC", "BTCUSDT", "ETHUSDT"]
        }
    }

Stream Data Segment
"""""""""""""""""""

.. csv-table:: Stream Data Segment
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    :escape: \
    
    "event", "String", "**MUST** be ``order``"
    "data.id", "Integer", "Order id"
    "data.clt_ord_id", "String" , "Client order id"
    "data.user_id", "Integer", "User id"
    "data.symbol", "String", "Symbol name"
    "data.side", "String", "Order side, refer to :doc:`Side <./dict>`"
    "data.price", "String", "Order price"
    "data.quantity", "String", "Order quantity"
    "data.type", "String", "Order type.Refer to :doc:`OrderType <./dict>`"
    "data.status", "String", "Refer to :doc:`OrderStatus <./dict>`"
    "data.create_time", "Integer", "Order create UNIX timestampe, in millisecond"
    "data.update_time", "Integer", "Order last update UNIX timestampe, in millisecond"

.. code-block:: json
   :caption: Stream Data Segment

    {
        "event": "order",
        "data": {
            "id": 129699,
            "clt_ord_id": "5evl5jwtij01etnl",
            "user_id": 112,
            "symbol": "BTCUSDT",
            "side": "bid",
            "price": "5000.00",
            "quantity": "0.001000",
            "type": "limit",
            "status": "submitted",
            "create_time": 1537346982470867,
            "update_time": 1537346982470914
        }
    }

Account
^^^^^^^

Request
"""""""

.. csv-table:: Request Data
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "**MUST** be ``subscribe``"
    "data.topic", "String", "**MUST** be ``account``"

.. code-block:: json
   :caption: Request All Symbol Orders

    {
        "event": "subscribe",
        "data": {
            "topic": "account"
        }
    }

Response
""""""""

.. csv-table:: Response Data
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80

    "event", "String", "**MUST** be ``subscribed``"
    "data.topic", "String", "**MUST** be ``account``"

.. code-block:: json
   :caption: Response Data

    {
        "event": "subscribed",
        "data": {
            "topic": "account"
        }
    }


Stream Data Segment
"""""""""""""""""""

.. csv-table:: Stream Data Segment
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    :escape: \
    
    "event", "String", "**MUST** be ``account``"
    "data", "Array", "Account updates"
    "data[].currency", "String", "Refer to :doc:`Currency <./dict>`"
    "data[].available", "String", "Account available balance for trading and withdraw"
    "data[].locked", "String", "Account locked balance for trading and deposit"
    "data[].timestamp", "Integer", "Account update UNIX timestamp, in millisecond"

.. code-block:: json
   :caption: Stream Data Segment

    {
        "event": "account",
        "data": [{
                "currency": "USDT",
                "available": "99991281.13",
                "locked": "5.01",
                "timestamp": 1537346985001
            }
        ]
    }

