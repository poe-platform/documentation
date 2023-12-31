# Sending files with your bot

With the fastapi\_poe library, send file attachments with your bot response by calling `post_message_attachment` within the `get_response` function of your bot.

```python
post_message_attachment(
    self, 
    access_key, 
    message_id, 
    download_url=None, 
    file_data=None, 
    filename=None,
)
```

**Parameters**:

* **access\_key** - the access key for your bot.
* **message\_id** - the message id associated with the current`QueryRequest`object being processed. **Important**: This must be the request that is currently being handled by `get_response`. Attempting to attach files to previously handled requests will fail.

Additionally, the following must be provided

* **download\_url** - provide a url to a file that will be attached to the message.

OR

* **file\_data** - the contents of the file to be uploaded. This should be a bytes-like or file object.&#x20;
* **filename** - the name of the file to be attached.&#x20;



**Example**:

In this example, the bot will take the input from the user, write it into a text file, and attach that text file in the response to the user.

```python
async def get_response(
    self, query: QueryRequest
) -> AsyncIterable[Union[PartialResponse, ServerSentEvent]]:
    last_message = query.query[-1].content

    with open("example.txt", "w") as file:
        file.write(last_message)

    with open("example.txt", "rb") as file:
        file_data = file.read()

    access_key = "BOT_ACCESS_KEY"
    # Attaches the example.txt file to the message
    await self.post_message_attachment(
        access_key, query.message_id, file_data=file_data, filename="example.txt"
    )
    # Attaches a file from URL to the message
    await self.post_message_attachment(
        access_key, query.message_id, download_url="https://www.google.com/test.png"
    )

    yield PartialResponse(text="I have attached your input in a text file!")
```



**Notes:**

* The `access_key` should be the key associated with the bot sending the response. It can be found in the edit bot page.
* It does not matter where `post_message_attachment` is called, as long as it is within the body of `get_response`. It can be called multiple times to attach multiple (up to 20) files.
* Each file should not be larger than 50mb.

