# Sending files with your bot

With the fastapi\_poe library, send file attachments with your bot response by calling `post_message_attachment` within the `get_response` function of your bot.

```python
post_message_attachment(self, query_request, file_data, filename, access_key)
```

**Parameters**:

* **query\_request** - the current`QueryRequest`object being processed. **Important**: This must be the request that is currently being handled by `get_response`. Attempting to attach files to previously handled requests will fail.
* **file\_data** - the contents of the file to be uploaded. This should be a bytes-like or file object.
* **filename** - the name of the file to be attached.
* **access\_key** - the access key for your bot.



**Example**:

In this example, the bot will take the input from the user, write it into a text file, and attach that text file in the response to the user.

```python
async def get_response(
    self, query: QueryRequest
) -> AsyncIterable[Union[PartialResponse, ServerSentEvent]]:
    last_message = query.query[-1].content

    with open("what_you_wrote.txt", "w") as file:
        file.write(last_message)

    with open("what_you_wrote.txt", "rb") as file:
        file_data = file.read()

    access_key = "BOT_ACCESS_KEY"
    await self.post_message_attachment(
        query.message_id, file_data, "what_you_wrote.txt", access_key
    )

    yield PartialResponse(text="I have attached your input in a text file!")
```



**Notes:**

* The `access_key` should be the key associated with the bot sending the response. It can be found in the edit bot page.
* It does not matter where `post_message_attachment` is called, as long as it is within the body of `get_response`.
* There can be up to 20 files attached per response. Each file should not be larger than 50mb.

