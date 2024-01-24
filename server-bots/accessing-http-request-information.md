# Accessing HTTP request information

Our python client ([fastapi\_poe](https://github.com/poe-platform/fastapi\_poe)) exposes the underlying Starlette [Request](https://www.starlette.io/requests/) object in the ".http\_request" attribute of the request object passed to the query handler. This allows you to access the request information such as the url and query params. The following is an example (including the setup code you need to host this on [Modal](https://modal.com/)):

```python
from __future__ import annotations

from typing import AsyncIterable

import fastapi_poe as fp
from modal import Image, Stub, asgi_app


class HttpRequestBot(fp.PoeBot):
    async def get_response_with_context(
        self, request: fp.QueryRequest, context: fp.RequestContext
    ) -> AsyncIterable[fp.PartialResponse]:
        request_url = context.http_request.url
        query_params = context.http_request.query_params
        yield fp.PartialResponse(
            text=f"The request url is: {request_url}, query params are: {query_params}"
        )


REQUIREMENTS = ["fastapi-poe==0.0.31"]
image = Image.debian_slim().pip_install(*REQUIREMENTS)
stub = Stub("http-request")


@stub.function(image=image)
@asgi_app()
def fastapi_app():
    bot = HttpRequestBot()
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

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>
