# Samples and next steps

## Sample

[![Screenshot of a Poe chat where the user asks "What is the capital of Nepal?" and the bot answers "The capital of Nepal is Kathmandu".](https://github.com/poe-platform/poe-protocol/raw/main/images/chat-sample.png)](https://github.com/poe-platform/poe-protocol/blob/main/images/chat-sample.png)

Suppose we’re having the above conversation over Poe with a bot server running at `https://ai.example.com/llm`.

For the Poe conversation above, the Poe server sends a POST request to `https://ai.example.com/llm` with the following JSON in the body:

```json
{
    "version": "1.0",
    "type": "query",
    "query": [
        {
            "role": "user",
            "content": "What is the capital of Nepal?",
            "content_type": "text/markdown",
            "timestamp": 1678299819427621,
        }
    ],
    "user": "u-1234abcd5678efgh",
    "conversation": "c-jklm9012nopq3456",
}
```

The bot server responds with an HTTP 200 response code, then sends the following server-sent events:

```json
event: meta
data: {"content_type": "text/markdown", "linkify": true}

event: text
data: {"text": "The"}

event: text
data: {"text": " capital of Nepal is"}

event: text
data: {"text": " Kathmandu."}

event: done
data: {}
```

The text may also be split into more or fewer individual events as desired. Sending more events means that users will see partial responses from the bot server faster.

## Next steps

* Check out our [quick start](https://github.com/poe-platform/server-bot-tutorial) to promptly get a bot running.
* Example implementations
  * [HerokuCat](https://poe.com/HerokuCat), a demo bot to demonstrate the features of the protocol.
    * See the [documentation](https://github.com/poe-platform/server-bot-tutorial/blob/main/catbot/catbot.md) for a full list of commands supported.
    * The source code for this bot is available in the [tutorial](https://github.com/poe-platform/server-bot-tutorial/blob/main/catbot/\_\_init\_\_.py).
  * [fastapi-poe](https://github.com/poe-platform/poe-protocol/blob/main/fastapi\_poe), a library for building Poe bots using the FastAPI framework. We recommend using this library if you are building your own bot.
  * [aiohttp-poe](https://github.com/poe-platform/poe-protocol/blob/main/aiohttp\_poe), a similar library built on top of aiohttp
  * [langchain-poe](https://github.com/poe-platform/poe-protocol/blob/main/langchain\_poe), an example bot built on top of ChatGPT using [LangChain](https://github.com/hwchase17/langchain)
  * [llama-poe](https://github.com/poe-platform/poe-protocol/blob/main/llama\_poe), a knowledge-augmented Poe bot powered by [LlamaIndex](https://gpt-index.readthedocs.io/en/latest/) and FastAPI.
  * [Poe Simulator](https://github.com/poe-platform/poe-protocol/blob/main/simulator\_poe), a simulated Poe server for testing your botjson
