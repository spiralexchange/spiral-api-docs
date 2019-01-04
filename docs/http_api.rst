HTTP REST API
=============

Base URI: https://api.spiral.exchange/api/v1

Refer to :ref:`Request and Response <REST-API-FORMAT>` for request and response data structure and encoding.

Public API
----------

.. Note::
    
    Public API **DOES NOT** require authentication.

GET /currencies
^^^^^^^^^^^^^^^

URI: https://api.spiral.exchange/api/v1/currencies

Fetch exchange currencies information

Response
""""""""

.. csv-table:: Response
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80

    "data", "Array", "Array of currency information"
    "data[].id", "Integer", "Currency Id"
    "data[].code", "String", "Currency code name"
    "data[].name", "String", "Currency name"
    "data[].precision", "Integer", "Max decimal places"
    "data[].can_deposit", "Boolean", "Whether spiral exchange supports deposit this currency"
    "data[].can_withdrawal", "Boolean", "Whether spiral exchange supports withdraw this currency"
    "data[].min_confirms", "Integer", "Minimal block chain confirms for deposit and withdraw operation"
    "data[].withdrawal_fee", "String", "Withdraw fee amount"
    "data[].withdraw_min_amount", "String", "Minimal withdraw amount"

.. code-block:: json
   :caption: Response

    {
        "data": [{
                "id": 10,
                "code": "USDT",
                "name": "USD",
                "precision": 2,
                "can_deposit": true,
                "can_withdrawal": true,
                "min_confirms": 6,
                "withdrawal_fee": "3",
                "withdraw_min_amount": "200"
            }, {
                "id": 20,
                "code": "BTC",
                "name": "BTC",
                "precision": 6,
                "can_deposit": true,
                "can_withdrawal": true,
                "min_confirms": 6,
                "withdrawal_fee": "0.001",
                "withdraw_min_amount": "0.01"
            }
        ]
    }


GET /products
^^^^^^^^^^^^^

URI: https://api.spiral.exchange/api/v1/products

Fetch exchange production information.


Response
""""""""

.. csv-table:: Response
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80

    "data", "Array", "Array of production information"
    "data[].symbol", "String", "Symbol name"
    "data[].quoteAssetName", "String", "Quote asset currency name"
    "data[].quoteAsset", "String", "Quote asset currency"
    "data[].quoteAssetUnit", "String", "Quote asset currency unit symbol char"
    "data[].baseAssetName", "String", "Base asset currency name"
    "data[].baseAsset", "String", "Base assert currency"
    "data[].baseAssetUnit", "String", "Base asset currency unit symbol char"
    "data[].active", "Boolean", "Whether production active"
    "data[].tickSize", "String", "Tick size"
    "data[].minTrade", "Number", "Minimal quantity"
    "data[].minQty", "Integer", "``Deprecated``, **DO NOT** use this field. Will be removed in futrue"
    "data[].status", "String", "Production status. Refer to :doc:`ProudctionStatus <./dict>`"

.. code-block:: json
   :caption: Response

    {
        "data": [{
                "symbol": "BTCUSDT",
                "quoteAssetName": "TetherUS",
                "baseAssetUnit": "฿",
                "baseAssetName": "Bitcoin",
                "baseAsset": "BTC",
                "tickSize": "0.01",
                "quoteAsset": "USDT",
                "quoteAssetUnit": "",
                "active": true,
                "minTrade": 0.000001,
                "status": "TRADING"
            }, {
                "symbol": "ETHBTC",
                "quoteAssetName": "Bitcoin",
                "baseAssetUnit": "Ξ",
                "baseAssetName": "Ethereum",
                "baseAsset": "ETH",
                "tickSize": "0.000001",
                "quoteAsset": "BTC",
                "quoteAssetUnit": "฿",
                "active": true,
                "minTrade": 0.001,
                "status": "TRADING"
            }
        ]
    }

GET /klines
^^^^^^^^^^^

URI: https://api.spiral.exchange/api/v1/klines

Fetch trading symbol kline data

Request
"""""""

.. csv-table:: Request
    :header: "Field Name", "Data Type", "Location", "Description"
    :widths: 20, 20, 20, 80

    "symbol", "String", "Query", "``REQUIRED`` Trading symbol name"
    "period", "Integer", "Query", "Period of K-Line, such as 1, 5, 15, in minute unit."
    "limit", "Integer", "Query", "Number of results to fetch. Range: 1 - 500"

Response
""""""""

.. csv-table:: Response
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    :escape: \

    "data", "Array of Array", "Kline datas"
    "data[][0]", "Integer", "Open time, UNIX timestamp, in milliseconds"
    "data[][1]", "String", "Open price"
    "data[][2]", "String", "High price"
    "data[][3]", "String", "Low price"
    "data[][4]", "String", "Close price"
    "data[][5]", "String", "Traded volumn"
    "data[][6]", "Integer", "Close time, UNIX timestamp, in millisecond"
    "data[][7]", "String", "Reserved future usage, always \"0\""
    "data[][8]", "Integer", "Number of trades"


.. code-block:: json
   :caption: Response

    {
        "data": [
            [1537381980000, "5345.17", "5345.17", "5345.17", "5345.17", "0", 1537382039000, "0", 0], 
            [1537382040000, "5345.17", "5345.17", "5345.17", "5345.17", "0", 1537382099000, "0", 0]
        ]
    }

GET /orderbook
^^^^^^^^^^^^^^

URI: https://api.spiral.exchange/api/v1/orderbook

Fetch trading symbol order book data

Request
"""""""

.. csv-table:: Request
    :header: "Field Name", "Data Type", "Location", "Description"
    :widths: 20, 20, 20, 80

    "symbol", "String", "Query", "``REQUIRED`` Trading symbol name"
    "limit", "Integer", "Query", "| ``REQUIRED`` Number of results to fetch. 
    | Min = 0, Max = 1000.  Valid values: ``0``, ``5``, ``10``, ``20``, ``50``, ``100``, ``1000``.
    | For limit = 0, full order book will be returned."

Response
""""""""

.. csv-table:: Response
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80

    "symbol", "String", "Symbol name"
    "last_update_id", "Integer", "Last update id"
    "data", "Array of Array", "Array of order book data"
    "data[][0]", "String", "Price"
    "data[][1]", "String", "Size"
    "data[][2]", "String", "Side. Refer to :doc:`Side <./dict>`"


.. code-block:: json
   :caption: Response

    {
        "symbol": "BTCUSDT",
        "last_update_id": 30105591,
        "data": [["6083.99", "0.034492", "bid"], ["6084.07", "0.007068", "bid"]]
    }

GET /trades
^^^^^^^^^^^

URI: https://api.spiral.exchange/api/v1/trades

Fetch trades

Request
"""""""

.. csv-table:: Request
    :header: "Field Name", "Data Type", "Location", "Description"
    :widths: 20, 20, 20, 80

    "symbol", "String", "Query", "Trading symbol name"
    "start", "Integer", "Query", "Offset of trades start to fetch"
    "count", "Integer", "Query", "``REQUIRED`` Number of trades to fetch. Range: 1 - 1000"
    "reverse", "Boolean", "Query", "Whether sort results in create time descend"
    "start_time", "Integer", "Query", "UNIX timestamp, in seconds. Filter trades whose create time after start_time"
    "end_time", "Integer", "Query", "UNIX timestamp, in seconds. Filter trades whose create time before end_time"

Response
""""""""

.. csv-table:: Response
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80

    "trades", "Array", "Array of trade data"
    "trades[].id", "Integer", "Trade id"
    "trades[].side", "String", "Trade side. Refer to :doc:`Side <./dict>`"
    "trades[].symbol", "String", "Product symbol"
    "trades[].price", "String", "Trade Price"
    "trades[].quantity", "String", "Trade quantity"
    "trades[].fee", "String", "Trade fee"
    "trades[].timestamp", "Interger", "Trade UNIX timestamp, in microseconds"

.. code-block:: json
   :caption: Response

    {
        "trades": [{
                "id": 143509701,
                "side": "ask",
                "symbol": "BTCUSDT",
                "price": "4810.65",
                "quantity": "0.028000",
                "fee": "0",
                "timestamp": 1537413338447303
            }, {
                "id": 143509601,
                "side": "ask",
                "symbol": "BTCUSDT",
                "price": "4810.65",
                "quantity": "0.028000",
                "fee": "0",
                "timestamp": 1537413326823342
            }
        ]
    }


Private API
-----------

.. Note::

    Private API **REQUIRES** authentication. Refer to :doc:`./authentication`


GET /wallet/balances
^^^^^^^^^^^^^^^^^^^^

URI: https://api.spiral.exchange/api/v1/wallet/balances

Fetch account wallet balances

Request
"""""""

.. csv-table:: Request
    :header: "Field Name", "Data Type", "Location", "Description"
    :widths: 20, 20, 20, 80

    "currency", "String", "Query", "Fetch specified wallet balance. Fetch all wallet balances if empty."

Response
""""""""

.. csv-table:: Response
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80

    "data", "Array", "Array of wallet balance"
    "data[].currency", "String", "Currency name"
    "data[].available", "String", "Available balance to trading and withdraw"
    "data[].locked", "String", "Locked balance of in trading and deposit"
    "data[].timestamp", "Interger", "Last update UNIX timestamp, in milliseconds"


.. code-block:: json
   :caption: Response

    {
        "data": [{
                "currency": "USDT",
                "available": "22742.94179847856",
                "locked": "76573.14",
                "timestamp": 1537148044221
            }, {
                "currency": "BTC",
                "available": "998.1",
                "locked": "2",
                "timestamp": 1537174046295
            }
        ]
    }

GET /myTrades
^^^^^^^^^^^^^

URI: https://api.spiral.exchange/api/v1/myTrades

Fetch private trades

Request
"""""""

.. csv-table:: Request
    :header: "Field Name", "Data Type", "Location", "Description"
    :widths: 20, 20, 20, 80

    "symbol", "String", "Query", "Trading symbol name"
    "start", "Integer", "Query", "Offset of trades start to fetch"
    "count", "Integer", "Query", "``REQUIRED`` Number of trades to fetch. Range: 1 - 1000"
    "reverse", "Boolean", "Query", "Whether sort results in create time descend"
    "start_time", "Integer", "Query", "UNIX timestamp, in seconds. Filter trades whose create time after start_time"
    "end_time", "Integer", "Query", "UNIX timestamp, in seconds. Filter trades whose create time before end_time"

Response
""""""""

.. csv-table:: Response
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80

    "trades", "Array", "Array of trade data"
    "trades[].id", "Integer", "Trade id"
    "trades[].side", "String", "Trade side. Refer to :doc:`Side <./dict>`"
    "trades[].symbol", "String", "Product symbol"
    "trades[].price", "String", "Trade Price"
    "trades[].quantity", "String", "Trade quantity"
    "trades[].fee", "String", "Trade fee"
    "trades[].timestamp", "Interger", "Trade UNIX timestamp, in microseconds"

.. code-block:: json
   :caption: Response

    {
        "trades": [{
                "id": 143509701,
                "side": "ask",
                "symbol": "BTCUSDT",
                "price": "4810.65",
                "quantity": "0.028000",
                "fee": "0",
                "timestamp": 1537413338447303
            }, {
                "id": 143509601,
                "side": "ask",
                "symbol": "BTCUSDT",
                "price": "4810.65",
                "quantity": "0.028000",
                "fee": "0",
                "timestamp": 1537413326823342
            }
        ]
    }

GET /order
^^^^^^^^^^

URI: https://api.spiral.exchange/api/v1/order

Fetch private orders

Rquest
""""""

.. csv-table:: Request
    :header: "Field Name", "Data Type", "Location", "Description"
    :widths: 20, 20, 20, 80

    "symbol", "String", "Query", "Trading symbol name"
    "side", "String", "Query", "Side. Refer to :doc:`Side <./dict>`"
    "filter", "String", "Query", "| JSON string. Generic table filter. Supported filters:
    | ``open``: Boolean, query open or close orders"
    "count", "Integer", "Query", "``REQUIRED`` Number of orders to fetch. Range: 1 - 1000"
    "reverse", "Boolean", "Query", "Whether sort results in create time descend"
    "start_time", "Integer", "Query", "UNIX timestamp, in microseconds. Filter trades whose create time after start_time"
    "end_time", "Integer", "Query", "UNIX timestamp, in microseconds. Filter trades whose create time before end_time"

Response
""""""""

.. csv-table:: Response
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80

    "orders", "Array", "Array of order data"
    "orders[].id", "Integer", "Trade id"
    "orders[].clt_ord_id", "String", "Client order id"
    "orders[].symbol", "String", "Symbol name"
    "orders[].side", "String", "Side. Refer to :doc:`Side <./dict>`"
    "orders[].price", "String", "Order price"
    "orders[].filled_price", "String", "Order filled price"
    "orders[].quantity", "String", "Order quantity"
    "orders[].filled_quantity", "String", "order filled quantity"
    "orders[].type", "String", "Order type. Refer to :doc:`OrderType <./dict>`"
    "orders[].status", "String", "Order status. Refer to :doc:`OrderStatus <./dict>`"
    "orders[].create_time", "Integer", "Create time, UNIX timestamp, in microseconds"
    "orders[].update_time", "Integer", "Last update time, UNIX timestamp, in microseconds"

.. code-block:: json
   :caption: Response

    {
        "orders": [{
                "id": 74019,
                "clt_ord_id": "l7ng1sarzxs5qc5y",
                "symbol": "BTCUSDT",
                "side": "ask",
                "price": "6.39",
                "filled_price": "0.00",
                "quantity": "0.000001",
                "filled_quantity": "0.000000",
                "type": "limit",
                "status": "cancelled",
                "create_time": 1537173156887047,
                "update_time": 1537174045391197
            }
        ]
    }

POST /order
^^^^^^^^^^^

URI: https://api.spiral.exchange/api/v1/order

Place order

Request
"""""""

.. csv-table:: Request
    :header: "Field Name", "Data Type", "Location", "Description"
    :widths: 20, 20, 20, 80

    "clt_ord_id", "String", "Body", "| Client Order ID. This id will come back on the order and any related executions.
    | Should only contains alphanumeric characters, underscore, hyphen and colon"
    "symbol", "String", "Body", "``REQUIRED`` Symbol name"
    "type", "String", "Body", "``REQUIRED`` Refer to :doc:`OrderType <./dict>`"
    "price", "String", "Body", "``REQUIRED`` Order price"
    "quantity", "String", "Body", "``REQUIRED`` Order quantity"
    "side", "String", "Body", "``REQUIRED`` Refer to :doc:`Side <./dict>`"

.. code-block:: json
   :caption: Request

    {
        "symbol": "BTCUSDT",
        "side": "bid",
        "type": "limit",
        "quantity": "0.01",
        "price": "2800"
    }

Response
""""""""

.. csv-table:: Response
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80

    "order", "Object", "Order object"
    "order.id", "Integer", "Order id"
    "order.clt_ord_id", "String", "Order client id"
    "order.user_id", "Integer", "Order user id"
    "order.symbol", "String", "Symbol name"
    "order.side", "String", "Refer to :doc:`Side <./dict>`"
    "order.price", "String", "Order price"
    "order.quantity", "String", "Order quantity"
    "order.type", "String", "Refer to :doc:`OrderType <./dict>`"
    "order.status", "String", "Refer to :doc:`OrderStatus <./dict>`"
    "order.create_time", "Integer", "Order create time, UNIX timestamp, in microseconds"
    "order.update_time", "Integer", "Order update time, UNIX timestamp, in microseconds"

.. code-block:: json
   :caption: Response

    {
        "order": {
            "id": 131513,
            "clt_ord_id": "9tiw3c4lbi1vatwt",
            "user_id": 432,
            "symbol": "BTCUSDT",
            "side": "bid",
            "price": "2800",
            "quantity": "0.000001",
            "type": "limit",
            "status": "submitted",
            "create_time": 1537428206100173,
            "update_time": 1537428206100173
        }
    }


DELETE /order
^^^^^^^^^^^^^

URI: https://api.spiral.exchange/api/v1/order

Cancel specified order

Request
"""""""

.. csv-table:: Request
    :header: "Field Name", "Data Type", "Location", "Description"
    :widths: 20, 20, 20, 80

    "order_id", "String", "Query", "``REQUIRED`` Order id to cancel"

Response
""""""""

Empty object

.. code-block:: json
   :caption: Response

    {}

DELETE /order/all
^^^^^^^^^^^^^^^^^

URI: https://api.spiral.exchange/api/v1/order/all

Cancel all orders

Request
"""""""

.. csv-table:: Request
    :header: "Field Name", "Data Type", "Location", "Description"
    :widths: 20, 20, 20, 80

    "symbol", "String", "Query", "Cancel orders of specified symbol"
    "filter", "String", "Query", "| JSON string. Generic table filter. Supported filters:
    | ``side``: String, ``buy`` or ``sell``"

Response
""""""""

Empty object

.. code-block:: json
   :caption: Response

    {}
