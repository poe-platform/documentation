# Setting an introduction message

The Poe API allows you to set a friendly introduction message for your bot, providing you with a way to instruct the users on how they should use the bot. In order to do so, you have to override `get_settings` and set the parameter called `introduction_message` to whatever you want that message to be.

```python
    async def get_settings(self, setting: SettingsRequest) -> SettingsResponse:
        return SettingsResponse(
            introduction_message="Welcome to the trivia bot. Please provide me a topic that you would like me to quiz you on."
        )
```

The final code (including the setup code you need to host this on [Modal](https://modal.com/)) that goes into our `main.py` is as follows:

```python
from __future__ import annotations
from typing import AsyncIterable
from modal import Image, Stub, asgi_app
from fastapi_poe import PoeBot, make_app
from fastapi_poe.types import (
    PartialResponse,
    QueryRequest,
    SettingsRequest,
    SettingsResponse,
)

class TriviaBotSample(PoeBot):
    async def get_response(self, query: QueryRequest) -> AsyncIterable[PartialResponse]:
        # implement the trivia bot.
        yield self.text_event("Bot under construction. Please visit later")

    async def get_settings(self, setting: SettingsRequest) -> SettingsResponse:
        return SettingsResponse(
            introduction_message="Welcome to the trivia bot. Please provide me a topic that you would like me to quiz you on."
        )
    
REQUIREMENTS = ["fastapi-poe==0.0.23"]
image = Image.debian_slim().pip_install(*REQUIREMENTS)
stub = Stub("trivia-test-poe-bot")

@stub.function(image=image)
@asgi_app()
def fastapi_app():
    bot = TriviaBotSample()
    app = make_app(bot, allow_without_key=True)
    return app
```

To learn how to setup Modal, please follow Steps 1 and 2 in our [Quick start](quick-start.md). If you already have Modal set up, simply run `modal deploy main.py`. Modal will then deploy your bot server to the cloud and output the server url. Use that url when creating a server bot on [Poe](https://poe.com/create\_bot?server=1). Once your bot is up, update your bot's settings (one time only after you override `get_settings`) by following [this](updating-bot-settings.md) guide. That's it, your bot is now ready.

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>
