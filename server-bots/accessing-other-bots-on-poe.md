# Accessing other bots on Poe

The Poe third party bot API allows developers to invoke other bots on Poe (which includes bots created by Poe like GPT-3.5-Turbo and Claude-Instant and bots created by other developers) and this access is provided for free so that developers do not have to worry about LLM costs. For every user message, server bot developers get to make up to two calls to another bot of their choice.

{% hint style="info" %}
If you are just getting started with server bots, we recommend checking out our [quick start](quick-start.md) guide. The following tutorial is specifically for how you invoke other bots and assumes that you are familiar with the concept of server bots.
{% endhint %}

### Install the Poe FastAPI client

```bash
pip install fastapi_poe
```

### Implement the PoeBot class&#x20;

#### Define what bots you want to use

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
