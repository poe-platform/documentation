# Requests

The Poe server will send an HTTP POST request to the bot servers URL with content type `application/json`. The body will be a JSON dictionary with the following keys:

* `version` _(string)_: The API version that the server is using.
* `type` _(string)_: This is one of the following strings:
  * `query`: Called when the user makes a query to the bot (i.e., they send a message).
  * `settings`: Query the bot for its desired settings.
  * `report_feedback`: Report to the bot server when certain events happen (e.g., the user likes a message).
  * `report_error`: Report to the bot server when an error happens that is attributable to the bot (e.g., it uses the protocol incorrectly).
  * Additional request types may be added in the future. Bot servers should ignore any request types they do not understand, ideally by sending a `501 Not Implemented` HTTP response.

Each of the request types is discussed in detail below:



<table data-view="cards"><thead><tr><th align="center"></th><th data-hidden></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td align="center"><strong>query</strong></td><td><em>request type</em></td><td><a href="query.md">query.md</a></td></tr><tr><td align="center"><strong>settings</strong></td><td><em>request type</em></td><td><a href="settings.md">settings.md</a></td></tr><tr><td align="center"><strong>report_feedback</strong></td><td><em>request type</em></td><td><a href="report_feedback.md">report_feedback.md</a></td></tr><tr><td align="center"><strong>report_error</strong></td><td><em>request type</em></td><td><a href="report_error.md">report_error.md</a></td></tr></tbody></table>

