.. caution::
    Authentication is still in developmenting.


Authentication
==============

There are two kinds of data: public and private.

Authentication is not needed for public data. There is no extra HTTP header or 
websocket request needed to query such data. Such as symbol and currencies information for HTTP REST API,
Market data for websocket.

Private data is authentication required. Such as orders, trades update.

API Key
-------

For private data, API key is used to authenticate and authorizate requests.

User can apply API key in spiral exchange account page.

After application, there are pair of keys generated: access key and secret key.

HTTP REST API
-------------

Authenticating with an API Key
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Authentication is done by sending the following HTTP headers:

*api-key*: Your access API key. This the id param returned when you create an API Key via the API.

*api-expires*: A UNIX timestamp after which the request is no longer valid. This is to prevent replay attacks.

``UNIX timestamps are in seconds. For example, 2018-02-08T04:30:37Z is 1518064237.``

*api-signature*: A signature of the request you are making.
It is calculated as *hex(HMAC_SHA256(apiSecret, verb + path + data))*

The *data* Param
^^^^^^^^^^^^^^^^
The data part of the HMAC construction should be exactly equal to the raw body you send to the server.
You can send JSON or form encoding, just ensure you use the exact same body string in the HMAC.
Generally you will want to prepare the request in your language of choice,
then use the same raw body string for the HMAC construction as in the request body.

Full sample calculation
^^^^^^^^^^^^^^^^^^^^^^^

Use these calculations as test cases in your code.

.. code-block:: python

    apiKey = 'LAqUlngMIQkIUjXMUreyu3qn'
    apiSecret = 'chNOOS4KvNXR_Xq4k4c9qsfoKWvnDecLATCRlcBwyKDYnWgO'

    #
    # Simple GET
    #
    verb = 'GET'
    # Note url-encoding on querystring - this is '/api/v1/instrument?filter={"symbol": "XBTM15"}'
    path = '/api/v1/instrument'
    expires = 1518064236 # 2018-02-08T04:30:36Z
    data = ''

    # HEX(HMAC_SHA256(apiSecret, 'GET/api/v1/instrument1518064236'))
    # Result is:
    # 'c7682d435d0cfe87c16098df34ef2eb5a549d4c5a3c2b1f0f77b8af73423bf00'
    signature = HEX(HMAC_SHA256(apiSecret, verb + path + str(expires) + data))

WebSocket
---------

After WebSocket connection established, client request *authenticate* event for authentication.

Signature is *hex(HMAC_SHA256(secret_key, 'GET/realtime' + expires))*

.. code-block:: json

    {
        "event": "authenticate",
        "data": {
            "api_key": "abc0de9fg8hi7jkl6mnop5qrst4uvw3x2y1z",
            "expires": 1521182920,
            "signature": "zyx1w2v3ut4sr5q6pon7ml8kji9hg0fedcba"
        }
    }