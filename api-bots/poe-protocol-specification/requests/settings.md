# settings

This request takes no additional request parameters other than the standard ones.

The server should respond with a response code of 200 and content type of `application/json`. The JSON response should be a dictionary containing the keys listed below. All keys are optional; if they are not specified the default values are used. Poe reserves the right to change the defaults at any time, so if bots rely on a particular setting, they should set it explicitly.

If a settings request fails (it does not return a 200 response code with a valid JSON body), the previous settings are used for the bot. If this is the first request, that means the default values are used for all settings; if it is a refetch request, the settings previously used for the bot remain in use. If the request does not return a 2xx or 501 response code, the Poe server may retry the settings request after some time.

### Response

The response may contain the following keys:

* `context_clear_window_secs` (integer or null): If the user does not talk to the bot for this many seconds, we clear the context and start a fresh conversation. Future `query` requests will use a new conversation identifier and will not include previous messages in the prompt. For example, suppose this parameter is set to 1800 (30 \* 60), and a user sends a message to a bot at 1 pm. If they then send another message at 1:20 pm, the query send to the bot server will include the previous message and response, but if they send a third message at 2 pm, the query will include only the new message. If this parameter is set to 0, the context window is never cleared automatically, regardless of how long it has been since the user talked to the bot. If the parameter is omitted or `null`, the value will be determined by the Poe server.
* `allow_user_context_clear` (boolean): If true, allow users to directly clear their context window, meaning that their conversation with this bot will reset to a clean slate. This is independent from whether Poe itself will automatically clear context after a certain time (as controlled by `context_clear_window_secs`). The default is true.
* `server_bot_dependencies` (mapping of strings to integers): Declare what bots this bot will access through the [bot connection API](../../api-to-access-bots-on-poe). The default is an empty mapping. The keys in the mapping are handles of Poe bots or the string "any", indicating that the bot may invoke any other bot. The values are the number of calls to each bot that the server bot is expected to make. For example, setting this field to `{"chatgpt": 1}` declares that the bot will use a single call to [ChatGPT](https://poe.com/ChatGPT), and setting it to `{"any": 2}` declares that the bot may make up to two calls to any other bot. Poe may show this value to users, and will enforce that bots do not access other bots that they did not declare beforehand.
* `allow_attachments` (boolean): If true, allow users to send attachments with messages sent to the bot. The default is false.
