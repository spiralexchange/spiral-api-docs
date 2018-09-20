Request and Response
====================

Data Encoding
--------------

All request and response data is in JSON format with UTF-8 encoding if not special statement.

.. _REST-API-FORMAT:

HTTP REST API
-------------

For ``GET`` request, parameters, if any, are encoded in ``x-www-form-urlencoded`` and sent via URL query string.

For ``POST`` and ``PUT`` request, request ``Content-Type`` must be ``application/json`` if no clarification.

Error Response
^^^^^^^^^^^^^^

HTTP Status: 200 OK

Content-Type: application/json

.. csv-table:: Error Response Fields
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "error_code", "Number", "Error code, 400 for bad request, 500 for server internal error, others for special meaning"
    "message", "String", "Detail error messages"

.. code-block:: json

    {
        "error_code": 400,
        "message": "Invalid request"
    }

Success Response
^^^^^^^^^^^^^^^^

HTTP Status: 200 OK

Content-type: application/json


There is no a unified structure of success response. They are different for each REST API.
To distinguish from error response, there is no ``error_code`` field in response data.


WebSocket
---------

Unified Data Structure
^^^^^^^^^^^^^^^^^^^^^^

Request and response uses unified JSON data structure.

.. csv-table:: WebSocket Field
    :header: "Field Name", "Data Type", "Description"
    :widths: 20, 20, 80
    
    "event", "String", "Event name, such as ping, authenticate and so on"
    "data", "N/A", "Event data. Data type is maybe string, number, boolean or object, depends on event definition."


.. code-block:: json

    {
        "event": "ping",
        "data": 1521182920
    }

Error Response
^^^^^^^^^^^^^^

``event`` field is "error", ``data`` field structure is the same as HTTP REST API error response.


.. code-block:: json

    {
        "event": "error",
        "data": {
            "code": 400,
            "message": "Bad Request"
        }
    }
