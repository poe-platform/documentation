# report\_error

When the bot server fails to use the protocol correctly (e.g., when it uses the wrong types in response to a `settings` request, the Poe server may make a `report_error` request to the server that reports what went wrong. The protocol does not guarantee that the endpoint will be called for every error; it is merely intended as a convenience to help bot server developers debug their bot.

This request takes the following additional parameters:

* `message` (string): A string describing the error.
* `metadata` (dictionary): May contain metadata that may be helpful in diagnosing the error, such as the conversation\_id for the conversation. The exact contents of the dictionary are not specified and may change at any time.

### Response

The server’s response is ignored.
