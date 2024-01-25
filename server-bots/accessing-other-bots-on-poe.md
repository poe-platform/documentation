# Accessing other bots on Poe

The Poe bot query API allows developers to invoke other bots on Poe (which includes bots created by Poe like GPT-3.5-Turbo and Claude-Instant and bots created by other developers) and this access is provided for free so that developers do not have to worry about LLM costs. For every user message, server bot developers get to make up to ten calls to another bot of their choice.

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
async def get_settings(self, setting: fp.SettingsRequest) -> fp.SettingsResponse:
    return fp.SettingsResponse(server_bot_dependencies={"GPT-3.5-Turbo": 1})
```

In your `get_response` handler, use the `stream_request` function to invoke any bot you want. The following is an example where we forward the user's query to `GPT-3.5-Turbo` and return the result.

```python
async def get_response(
    self, request: fp.QueryRequest
) -> AsyncIterable[fp.PartialResponse]:
    async for msg in fp.stream_request(
        request, "GPT-3.5-Turbo", request.access_key
    ):
        yield msg
```

The final code (including the setup code you need to host this on [Modal](https://modal.com/)) that goes into your `main.py` is as follows:

```python
from __future__ import annotations
from typing import AsyncIterable
from modal import Image, Stub, asgi_app
import fastapi_poe as fp

class GPT35TurboBot(fp.PoeBot):
    async def get_response(
        self, request: fp.QueryRequest
    ) -> AsyncIterable[fp.PartialResponse]:
        async for msg in fp.stream_request(
            request, "GPT-3.5-Turbo", request.access_key
        ):
            yield msg

    async def get_settings(self, setting: fp.SettingsRequest) -> fp.SettingsResponse:
        return fp.SettingsResponse(server_bot_dependencies={"GPT-3.5-Turbo": 1})
    
REQUIREMENTS = ["fastapi-poe==0.0.24"]
image = Image.debian_slim().pip_install(*REQUIREMENTS)
stub = Stub("turbo-example-poe")

@stub.function(image=image)
@asgi_app()
def fastapi_app():
    bot = GPT35TurboBot()
    app = fp.make_app(bot, allow_without_key=True)
    return app
```

To learn how to setup Modal, please follow Steps 1 and 2 in our [Quick start](quick-start.md). If you already have Modal set up, simply run `modal deploy main.py`. Modal will then deploy your bot server to the cloud and output the server url. Use that url when creating a server bot on [Poe](https://poe.com/create\_bot?server=1).&#x20;

Now, before you use the bot, you will have to follow the steps in [this](updating-bot-settings.md) article in order to get Poe to fetch your bots settings (one time only after you override `get_settings`). Once that is done, try to use your bot on Poe and you will see the response from GPT-3.5-Turbo. You can modify the code and do more interesting things (like apply some business logic on the response or conditionally call another API).

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

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

In your python shell, run the following after replacing the placeholder with your API key.

```python
import asyncio
import fastapi_poe as fp

# Create an asynchronous function to encapsulate the async for loop
async def get_responses(api_key, messages):
    async for partial in fp.get_bot_response(messages=messages, bot_name="GPT-3.5-Turbo", api_key=api_key):
        print(partial)
 
# Replace <api_key> with your actual API key, ensuring it is a string.
api_key = <api_key>
message = fp.ProtocolMessage(role="user", content="Hello world")

# Run the event loop
# For Python 3.7 and newer
asyncio.run(get_responses(api_key, [message]))

# For Python 3.6 and older, you would typically do the following:
# loop = asyncio.get_event_loop()
# loop.run_until_complete(get_responses(api_key))
# loop.close()
```

If you are using an ipython shell, you can instead use the following simpler code.

```python
import fastapi_poe as fp

message = fp.ProtocolMessage(role="user", content="Hello world")
async for partial in fp.get_bot_response(messages=[message], bot_name="GPT-3.5-Turbo", api_key=<api_key>): 
    print(partial)
```
