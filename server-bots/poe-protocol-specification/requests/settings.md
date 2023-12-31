# settings

This request takes no additional request parameters other than the standard ones.

The server should respond with a response code of 200 and content type of `application/json`. The JSON response should be a dictionary containing the keys listed below. All keys are optional; if they are not specified the default values are used. Poe reserves the right to change the defaults at any time, so if bots rely on a particular setting, they should set it explicitly.

If a settings request fails (it does not return a 200 response code with a valid JSON body), the previous settings are used for the bot. If this is the first request, that means the default values are used for all settings; if it is a refetch request, the settings previously used for the bot remain in use. If the request does not return a 2xx or 501 response code, the Poe server may retry the settings request after some time.

### Response

The response may contain the following keys:

* `server_bot_dependencies` (mapping of strings to integers): Declare what bots your server bot will access through the [bot query API](../../accessing-other-bots-on-poe.md). The keys in the mapping are handles of Poe bots and the values are the number of calls to each bot that the server bot is expected to make. For example, setting this field to `{"GPT-3.5-Turbo": 1}` declares that the bot will use a single call to [GPT-3.5-Turbo](https://poe.com/GPT-3.5-Turbo). Poe may show this value to users, and will enforce that bots do not access other bots that they did not declare beforehand. In addition, you may not currently use more than 10 calls to other bots for a single message.
* `allow_attachments` (boolean): If true, allow users to send attachments with messages sent to the bot. The default is false.
* `introduction_message` (string): Set a message that the bot will use to introduce itself to users at the start of a chat. This message is always treated as Markdown. By default, bots will have no introduction.
