# Sending files with your response

The Poe API allows you to send attachments with your bot response. When using the fastapi\_poe library, send file attachments with your bot response by calling `post_message_attachment` within the `get_response` function of your bot.

```python
async def post_message_attachment(
    self,
    message_id: Optional[Identifier] = None,
    *,
    download_url: Optional[str] = None,
    file_data: Optional[Union[bytes, BinaryIO]] = None,
    filename: Optional[str] = None,
    content_type: Optional[str] = None,
    is_inline: bool = False,
) -> AttachmentUploadResponse:
```

### **Parameters**

* **message\_id** - the message id associated with the current`QueryRequest`object being processed. **Important**: This must be the request that is currently being handled by `get_response`. Attempting to attach files to previously handled requests will fail.

Additionally, the following must be provided

* **download\_url** - provide a url to a file that will be attached to the message.

OR

* **file\_data** - the contents of the file to be uploaded. This should be a bytes-like or file object.&#x20;
* **filename** - the name of the file to be attached.&#x20;

### **Example**

In this example, the bot will take the input from the user, write it into a text file, and attach that text file in the response to the user. Copy the following code into a file called `main.py` (you can pick any name but the deployment commands that follow assume that this is the file name). Change the `access_key` stub with your actual key that you can generate on the [create bot](https://poe.com/create\_bot) page.

```python
from __future__ import annotations
from typing import AsyncIterable
import fastapi_poe as fp
from modal import Image, Stub, asgi_app


class AttachmentOutputDemoBot(fp.PoeBot):
    async def get_response(
        self, request: fp.QueryRequest
    ) -> AsyncIterable[fp.PartialResponse]:
        await self.post_message_attachment(
           message_id=request.message_id, file_data=request.query[-1].content, filename="dummy.txt"
        )
        yield fp.PartialResponse(text=f"Attached a text file containing your last message.")


REQUIREMENTS = ["fastapi-poe==0.0.34"]
image = Image.debian_slim().pip_install(*REQUIREMENTS)
stub = Stub("attachment-output-demo")


@stub.function(image=image)
@asgi_app()
def fastapi_app():
    bot = AttachmentOutputDemoBot(access_key="<put your access key here>")
    app = fp.make_app(bot)
    return app
```

To learn how to setup Modal, please follow Steps 1 and 2 in our [Quick start](quick-start.md). If you already have Modal set up, simply run `modal deploy main.py`. Modal will then deploy your bot server to the cloud and output the server url. Use that url when creating a server bot on [Poe](https://poe.com/create\_bot?server=1).

### **Notes**

* The `access_key` should be the key associated with the bot sending the response. It can be found in the edit bot page.
* It does not matter where `post_message_attachment` is called, as long as it is within the body of `get_response`. It can be called multiple times to attach multiple (up to 20) files.
* A file should not be larger than 50mb.
