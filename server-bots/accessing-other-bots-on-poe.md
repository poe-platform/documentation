# Accessing other bots on Poe

The Poe third party bot API allows developers to invoke other bots on Poe (which includes bots created by Poe like ChatGPT and Claude-Instant and bots created by other developers) and this access is provided for free so that developers do not have to worry about LLM costs. For every user message, server bot developers get to make up to two calls to another bot of their choice.

{% hint style="info" %}
If you are just getting started with server bots, we recommend checking out our [quick start](quick-start.md) guide. The following tutorial is specifically for how you invoke other bots and assumes that you are familiar with the concept of server bots.
{% endhint %}

#### Install the Poe FastAPI client

```bash
pip install fastapi_poe
```

#### Implement the PoeBot class&#x20;

You have to declare your bot dependencies using the [settings](poe-protocol-specification/requests/settings.md) endpoint. In get response, use the stream\_request function.

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


class ChatGPTBot(PoeBot):
    async def get_response(self, query: QueryRequest) -> AsyncIterable[PartialResponse]:
        async for msg in stream_request(query, "chatGPT", query.access_key):
            yield msg

    async def get_settings(self, setting: SettingsRequest) -> SettingsResponse:
        return SettingsResponse(
            server_bot_dependencies={"chatGPT": 1}
        )
```

The above response handler will invoke ChatGPT with the query passed by the user and return the result. You can modify the code and do more interesting things (like apply some business logic on the response or conditionally call another API).
