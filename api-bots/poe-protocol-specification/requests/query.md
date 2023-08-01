# query

### The `query` request type

In addition to the request fields that are valid for all queries, `query` requests take the following parameters in the request:

* `query`: An array containing one or more dictionaries that represent a previous message in the conversation with the bot. These are in chronological order, the most recent message last. It includes all messages in the current context window (see above). These dictionaries contain the following keys:
  * `role` (string): one of the following strings:
    * `system`: A message that tells the bot how it should work. Example: “You are an AI assistant that gives useful information to Poe users.”
    * `user`: A message from the user. Example: “What is the capital of Nepal?"
    * `bot`: A response from the bot. Example: “The capital of Nepal is Kathmandu.”
    * More roles may be added in the future. Bot servers should ignore messages with roles they do not recognize.
  * `content` (string): The text of the message.
  * `content_type` (string): The content type of the message (see under “Content type” above). Bots should ignore messages with content types they do not understand.
  * `timestamp` (int): The time the message was sent, as the number of microseconds since the Unix epoch.
  * `message_id` (identifier with type m): Identifier for this message.
  * `feedback` (array): A list of dictionaries representing feedback that the user gave to the message. Each dictionary has the following keys:
    * `type` (string): Either `like` or `dislike`. More types may be added in the future and bot servers should ignore types they do not recognize.
    * `reason` (string): A string representing the reason for the action. This key may be omitted.
  * `attachments` (array): A list of dictionaries representing attachments that the user has sent with message. Each dictionary has the following keys:
    * `url` (string): A URL pointing to the raw file. This URL is only guaranteed to remain valid for 10 minutes from when the request is sent.
    * `content_type` (string): The MIME type for the file (e.g., `image/png` or `application/pdf`).
    * `name` (string): The file name for the file (e.g., `paper.pdf`).
* `message_id` (identifier with type `m`): identifier for the message that the bot will create; also used for the `report_feedback` endpoint
* `user_id` (identifier with type `u`): the user making the request
* `conversation_id` (identifier with type `c`): identifier for the conversation the user is currently having. Resets when context is cleared.
* `metadata` (identifier with type `d`): internal metadata used by Poe when [accessing other bots](../../api-to-access-bots-on-poe.md). This data must be sent when using the API to access other Poe bots.

The Poe server may also send the following parameters influencing how the underlying
LLM, if any, is invoked. Bot servers may ignore these parameters or treat them as
hints as they wish:
* `temperature` (float in range `0 <= temperature <= infinity`): indicates what temperature the bot should use while making requests. Bots for which this setting does not make sense may ignore this parameter.
* `skip_system_prompt` (boolean): if set to true, bots should minimize any adjustments they make to the prompt before sending data to the underlying LLM. Exactly what this means is up to individual bots.
* `stop_sequences` (array of string): if the LLM encounters one of these strings, it should stop its response.
* `logit_bias` (object with float values): an object where the keys are tokens and the values are floats in the range `-100 <= value <= 100`, where a negative value makes the token less likely to be emitted and a positive value makes the token more likely to be emitted.

### Response

The bot server should respond with an HTTP response code of 200. If any other response code is returned, the Poe server will show an error message to the user. The server must respond with a stream of server-sent events, as specified by the WhatWG ([https://html.spec.whatwg.org/multipage/server-sent-events.html](https://html.spec.whatwg.org/multipage/server-sent-events.html)).

Server-sent events contain a type and data. The Poe API supports several event types with different meanings. For each type, the data is a JSON string as specified below. The following event types are supported:

* `meta`: represents metadata about how the Poe server should treat the bot server response. This event should be the first event sent back by the bot server. If no `meta` event is given, the default values are used. If a `meta` event is not the first event, the behavior is unspecified; currently it is ignored but future extensions to the protocol may allow multiple `meta` events in one response. The data dictionary may have the following keys:
  * `content_type` (string, defaults to `text/markdown`): If this is `text/markdown`, the response is rendered as Markdown by the Poe client. If it is `text/plain`, the response is rendered as plain text. Other values are unsupported and are treated like `text/plain`.
  * `linkify` (boolean, defaults to false): If this is true, Poe will automatically add links to the response that generate additional queries to the bot server.
  * `suggested_replies` (boolean, defaults to false): If this is true, Poe will suggest followup messages to the user that they might want to send to the bot. If this is false, no suggested replies will be shown to the user. Note that the protocol also supports bots sending their own suggested replies (see below). If the bot server sends any `suggested_reply` event, Poe will not show any of its own suggested replies, only those suggested by the bot, regardless of the value of the `suggested_replies` setting.
  * `refetch_settings` (boolean, defaults to false): Setting this to true advises the Poe server that it should refetch the `settings` endpoint and update the settings for this bot. Bot servers should set this to true when they wish to change their settings. The Poe server may not refetch settings for every response with this field set; for example, it may refetch only once per hour or day.
* `text`: represents a piece of text to send to the user. This is a partial response; the text shown to the user when the request is complete will be a concatenation of the texts from all `text` events. The data dictionary may have the following keys:
  * `text` (string): A partial response to the user’s query
* `replace_response`: like `text`, but discards all previous `text` events. The user will no longer see the previous responses, and instead see only the text provided by this event. The data dictionary must have the following keys:
  * `text` (string): A partial response to the user's query
* `suggested_reply`: represents a suggested followup query that the user can send to reply to the bot’s query. The Poe UI may show these followups as buttons the user can press to immediately send a new query. The data dictionary has the following keys:
  * `text` (string): Text of the suggested reply.
* `error`: indicates that an error occurred in the bot server. If this event type is received, the server will close the connection and indicate to the user that there was an error communicating with the bot server. The server may retry the request. The data dictionary may contain the following keys:
  * `allow_retry` (boolean): If this is False, the server will not retry the request. If this is True or omitted, the server may retry the request.
  * `text` (string): A message indicating more details about the error. This message will not be shown to the user, but Poe will use it for internal diagnostic purposes. May be omitted.
* `done`: must be the last event in the stream, indicating that the bot response is finished. The server will close the connection after this event is received. The data for this event is currently ignored, but it must be valid JSON.

The bot response must include at least one `text` or `error` event; it is an error to send no response.

If the Poe server receives an event type it does not recognize, it ignores the event.
