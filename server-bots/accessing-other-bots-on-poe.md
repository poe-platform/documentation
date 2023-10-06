# Accessing other bots on Poe

The Poe bot query API allows developers to invoke other bots on Poe (which includes bots created by Poe like GPT-3.5-Turbo and Claude-Instant and bots created by other developers) and this access is provided for free so that developers do not have to worry about LLM costs. For every user message, server bot developers get to make up to two calls to another bot of their choice.

{% hint style="info" %}
If you are just getting started with server bots, we recommend checking out our [quick start](quick-start.md) guide. The following tutorial is specifically for how you invoke other bots and assumes that you are familiar with the concept of server bots.
{% endhint %}

#### Install the Poe FastAPI client

```bash
pip install fastapi_poe
```

#### Implement the PoeBot class&#x20;

You have to declare your bot dependencies using the [settings](poe-protocol-specification/requests/settings.md) endpoint.&#x20;

```python
async def get_settings(self, setting: SettingsRequest) -> SettingsResponse:
    return SettingsResponse(
        server_bot_dependencies={"GPT-3.5-Turbo": 1}
    )
```

In your `get_response` handler, use the `stream_request` function to invoke any bot you want. The following is example where we `GPT-3.5-Turbo` with the query passed by the user and return the result.

```python
async def get_response(self, query: QueryRequest) -> AsyncIterable[PartialResponse]:
    async for msg in stream_request(query, "GPT-3.5-Turbo", query.access_key):
        yield msg
```

Your final code for this basic example should look like this:

```python
from typing import AsyncIterable

from fastapi_poe import PoeBot, run
from fastapi_poe.client import stream_request
from fastapi_poe.types import (
    PartialResponse,
    QueryRequest,
    SettingsRequest,
    SettingsResponse,
)


class GPT35TurboBot(PoeBot):
    async def get_response(self, query: QueryRequest) -> AsyncIterable[PartialResponse]:
        async for msg in stream_request(query, "GPT-3.5-Turbo", query.access_key):
            yield msg

    async def get_settings(self, setting: SettingsRequest) -> SettingsResponse:
        return SettingsResponse(
            server_bot_dependencies={"GPT-3.5-Turbo": 1}
        )
```

Now, before you use the bot, you will have to follow the steps in [this](updating-bot-settings.md) article in order to get Poe to fetch your bots settings. Once that is done, try to use your bot on Poe and you will see the response from GPT-3.5-Turbo. You can modify the code and do more interesting things (like apply some business logic on the response or conditionally call another API).

### How to access the bot query API directly

We also provide a helper function for you to test the bot query API in a lower friction manner.&#x20;

#### Install the Poe FastAPI client

```bash
pip install fastapi_poe
```

#### Get your API Key

Navigate to [poe.com/developers](https://poe.com/developers) and copy your user API key. Note that access to an API key is currently limited to Poe subscribers to minimize abuse.

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

Usage done with this API key will count against your user account's message limits on Poe, so be sure to only use it for testing and not for cases when other people are using your bot.

#### Access the bot query API using "get\_bot\_response"

{% code overflow="wrap" %}
```python
from fastapi_poe.types import ProtocolMessage
from fastapi_poe.client import get_bot_response

message = ProtocolMessage(role="user", content="Hello world")
async for partial in get_bot_response(messages=[message], bot_name="GPT-3.5-Turbo", api_key=<api_key>): 
    print(partial)
```
{% endcode %}
