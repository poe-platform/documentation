# Rendering an image in the response

The Poe API allows you to embed images in your bot's response using Markdown syntax. The following is an example implementation describing a bot that returns a static response containing an image.

```python
from typing import AsyncIterable
from modal import Image, Stub, asgi_app
import fastapi_poe as fp

IMAGE_URL = "https://images.pexels.com/photos/46254/leopard-wildcat-big-cat-botswana-46254.jpeg"

class SampleImageResponseBot(fp.PoeBot):
    async def get_response(
        self, request: fp.QueryRequest
    ) -> AsyncIterable[fp.PartialResponse]:
        yield fp.PartialResponse(text=f"This is a test image. ![leopard]({IMAGE_URL})")
    
REQUIREMENTS = ["fastapi-poe==0.0.24"]
image = Image.debian_slim().pip_install(*REQUIREMENTS)
stub = Stub("image-response-poe")

@stub.function(image=image)
@asgi_app()
def fastapi_app():
    bot = SampleImageResponseBot()
    app = fp.make_app(bot, allow_without_key=True)
    return app
```

In order to run this code, you will need to setup Modal. For that, please follow Steps 1 and 2 in our [Quick start](quick-start.md). If you already have Modal set up, simply run `modal deploy main.py`. Modal will then deploy your bot server to the cloud and output the server url. Use that url when creating a server bot on [Poe](https://poe.com/create\_bot?server=1).

The following is what the response looks like for someone using the above described bot.

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>
