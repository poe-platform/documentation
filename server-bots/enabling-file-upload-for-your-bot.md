# Enabling file upload for your bot

The Poe API allows you to enable file upload for your bot. This allows your bot to do more interesting things that would be possible using chat input alone. As part of this tutorial, we will create a very simple chatbot that takes in a pdf input and computes the number of pages in the pdf for the user.

To enable file upload, you have to override `get_settings` and set the parameter called `allow_attachments` to `True`

```python
    async def get_settings(self, setting: SettingsRequest) -> SettingsResponse:
        return SettingsResponse(allow_attachments=True)
```

Poe uploads any attachments provided by the user to its storage and passes the url of the file along with other metadata like name and type on to the bot server. We will utilize a python library called `pypdf2` (which you can install using `pip install pypdf2`) to parse the pdf and count the number of pages. We will use the `requests` library (which you can install using `pip install requests`) to download the file.

```python
def _fetch_pdf_and_count_num_pages(url: str) -> int:
    response = requests.get(url)
    if response.status_code != 200:
        raise FileDownloadError()
    with open("temp_pdf_file.pdf", "wb") as f:
        f.write(response.content)
    reader = PdfReader("temp_pdf_file.pdf")
    return len(reader.pages)
```

Now we will set up a bot class that will iterate through the user messages and identify the latest pdf file to compute the number of pages for.

```python
class PDFSizeBot(PoeBot):
    async def get_response(
        self, request: QueryRequest
    ) -> AsyncIterable[PartialResponse]:
        for message in reversed(request.query):
            for attachment in message.attachments:
                if attachment.content_type == "application/pdf":
                    try:
                        num_pages = _fetch_pdf_and_count_num_pages(attachment.url)
                        yield PartialResponse(text=f"{attachment.name} has {num_pages} pages")
                    except FileDownloadError:
                        yield PartialResponse(text="Failed to retrieve the document.")
                    return
```

The final code (including the setup code you need to host this on [Modal](https://modal.com/)) that goes into our `main.py` is as follows:

```python
from __future__ import annotations
from typing import AsyncIterable
import requests
from PyPDF2 import PdfReader

from modal import Image, Stub, asgi_app
from fastapi_poe import PoeBot, make_app
from fastapi_poe.types import (
    PartialResponse,
    QueryRequest,
    SettingsRequest,
    SettingsResponse,
)

class FileDownloadError(Exception):
    pass


def _fetch_pdf_and_count_num_pages(url: str) -> int:
    response = requests.get(url)
    if response.status_code != 200:
        raise FileDownloadError()
    with open("temp_pdf_file.pdf", "wb") as f:
        f.write(response.content)
    reader = PdfReader("temp_pdf_file.pdf")
    return len(reader.pages)


class PDFSizeBot(PoeBot):
    async def get_response(
        self, request: QueryRequest
    ) -> AsyncIterable[PartialResponse]:
        for message in reversed(request.query):
            for attachment in message.attachments:
                if attachment.content_type == "application/pdf":
                    try:
                        num_pages = _fetch_pdf_and_count_num_pages(attachment.url)
                        yield PartialResponse(text=f"{attachment.name} has {num_pages} pages")
                    except FileDownloadError:
                        yield PartialResponse(text="Failed to retrieve the document.")
                    return

    async def get_settings(self, setting: SettingsRequest) -> SettingsResponse:
        return SettingsResponse(allow_attachments=True)
    

image = Image.debian_slim().pip_install_from_requirements("requirements.txt")
stub = Stub("pdf-count-poe-bot")

@stub.function(image=image)
@asgi_app()
def fastapi_app():
    bot = PDFSizeBot()
    app = make_app(bot, allow_without_key=True)
    return app
```

The requirements.txt file is as follows:

```pip-requirements
fastapi-poe==0.0.23
pydantic>=2
PyPDF2==3.0.1
requests==2.31.0
```

To learn how to setup Modal, please follow Steps 1 and 2 in our [Quick start](quick-start.md). If you already have Modal set up, simply run `modal deploy main.py`. Modal will then deploy your bot server to the cloud and output the server url. Use that url when creating a server bot on [Poe](https://poe.com/create\_bot?server=1). Once your bot is up, update your bot's settings by following [this](updating-bot-settings.md) guide. That's it, your bot is now ready.

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>
