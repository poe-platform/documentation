# Accessing other bots on Poe

The Poe third party bot API allows bot developers to invoke other bots on Poe (which includes bots created by Poe like ChatGPT and Claude-Instant and bots created by other developers) and this access is provided for free so that developers do not have to worry about LLM costs. For every user message, server bot developers get to make up to two calls to another bot of their choice.

{% hint style="info" %}
If you are just getting started with server bots, we recommend checking out our [quick start](quick-start.md) guide. The following tutorial is specifically for how you invoke other bots and assumes that you are familiar with the concept of server bots.
{% endhint %}

#### Install the Poe FastAPI client

```bash
pip install fastapi_poe
```

#### Implement the PoeBot class and use the stream\_request function

```python
from fastapi_poe.client import stream_request

class ChatGPTBot(PoeBot):
    async def get_response(self, query: QueryRequest) -> AsyncIterable[ServerSentEvent]:
        async for msg in stream_request(query, "chatGPT", query.api_key):
            if isinstance(msg, MetaMessage):
                continue
            elif msg.is_suggested_reply:
                yield self.suggested_reply_event(msg.text)
            elif msg.is_replace_response:
                yield self.replace_response_event(msg.text)
            else:
                yield self.text_event(msg.text)
```

The above response handler will invoke chatGPT with the query passed by the user and return the result. You can change the above code and do more interesting things (like apply some business logic on the response or conditionally call another api).
