# Using OpenAI function calling

The Poe API allows you to use OpenAI function calling when accessing OpenAI models. In order to use this feature, you will simply need to provide a tools list which contains objects describing your function and an executables list which contains functions that correspond to the tools list. The following is an example.

```python
def get_current_weather(location, unit="fahrenheit"):
    """Get the current weather in a given location"""
    if "tokyo" in location.lower():
        return json.dumps({"location": "Tokyo", "temperature": "11", "unit": unit})
    elif "san francisco" in location.lower():
        return json.dumps(
            {"location": "San Francisco", "temperature": "72", "unit": unit}
        )
    elif "paris" in location.lower():
        return json.dumps({"location": "Paris", "temperature": "22", "unit": unit})
    else:
        return json.dumps({"location": location, "temperature": "unknown"})


tools_executables = [get_current_weather]

tools_dict_list = [
    {
        "type": "function",
        "function": {
            "name": "get_current_weather",
            "description": "Get the current weather in a given location",
            "parameters": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "The city and state, e.g. San Francisco, CA",
                    },
                    "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]},
                },
                "required": ["location"],
            },
        },
    }
]
tools = [fp.ToolDefinition(**tools_dict) for tools_dict in tools_dict_list]
```

Additionally, you will need to define a dependency of two calls on an OpenAI model of your choice (in this case, the GPT-3.5-Turbo). You need a dependency of two because as part of the OpenAI function calling [flow](https://platform.openai.com/docs/guides/function-calling/common-use-cases), you need to call OpenAI twice. Adjust this dependency limit if you want to make more than one function calling request while computing your response.

```python
async def get_settings(self, setting: fp.SettingsRequest) -> fp.SettingsResponse:
    return fp.SettingsResponse(server_bot_dependencies={"GPT-3.5-Turbo": 2})
```

The final code (including the setup code you need to host this on [Modal](https://modal.com/)) that goes into your `main.py` is as follows:

```python
from __future__ import annotations

import json
from typing import AsyncIterable

import fastapi_poe as fp
from modal import Image, Stub, asgi_app


def get_current_weather(location, unit="fahrenheit"):
    """Get the current weather in a given location"""
    if "tokyo" in location.lower():
        return json.dumps({"location": "Tokyo", "temperature": "11", "unit": unit})
    elif "san francisco" in location.lower():
        return json.dumps(
            {"location": "San Francisco", "temperature": "72", "unit": unit}
        )
    elif "paris" in location.lower():
        return json.dumps({"location": "Paris", "temperature": "22", "unit": unit})
    else:
        return json.dumps({"location": location, "temperature": "unknown"})


tools_executables = [get_current_weather]

tools_dict_list = [
    {
        "type": "function",
        "function": {
            "name": "get_current_weather",
            "description": "Get the current weather in a given location",
            "parameters": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "The city and state, e.g. San Francisco, CA",
                    },
                    "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]},
                },
                "required": ["location"],
            },
        },
    }
]
tools = [fp.ToolDefinition(**tools_dict) for tools_dict in tools_dict_list]


class GPT35FunctionCallingBot(fp.PoeBot):
    async def get_response(
        self, request: fp.QueryRequest
    ) -> AsyncIterable[fp.PartialResponse]:
        async for msg in fp.stream_request(
            request,
            "GPT-3.5-Turbo",
            request.access_key,
            tools=tools,
            tool_executables=tools_executables,
        ):
            yield msg

    async def get_settings(self, setting: fp.SettingsRequest) -> fp.SettingsResponse:
        return fp.SettingsResponse(server_bot_dependencies={"GPT-3.5-Turbo": 2})


REQUIREMENTS = ["fastapi-poe==0.0.25"]
image = Image.debian_slim().pip_install(*REQUIREMENTS)
stub = Stub("function-calling-poe")


@stub.function(image=image)
@asgi_app()
def fastapi_app():
    bot = GPT35FunctionCallingBot()
    # Optionally, provide your Poe access key here:
    # 1. You can go to https://poe.com/create_bot?server=1 to generate an access key.
    # 2. We strongly recommend using a key for a production bot to prevent abuse,
    # but the starter examples disable the key check for convenience.
    # 3. You can also store your access key on modal.com and retrieve it in this function
    # by following the instructions at: https://modal.com/docs/guide/secrets
    # POE_ACCESS_KEY = ""
    # app = make_app(bot, access_key=POE_ACCESS_KEY)
    app = fp.make_app(bot, allow_without_key=True)
    return app
```

To learn how to setup Modal, please follow Steps 1 and 2 in our [Quick start](quick-start.md). If you already have Modal set up, simply run `modal deploy main.py`. Modal will then deploy your bot server to the cloud and output the server url. Use that url when creating a server bot on [Poe](https://poe.com/create\_bot?server=1). Once your bot is up, update your bot's settings (one time only after you override `get_settings`) by following [this](updating-bot-settings.md) guide. That's it, your bot is now ready.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>
