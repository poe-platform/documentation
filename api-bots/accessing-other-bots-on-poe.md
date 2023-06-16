# Accessing other bots on Poe

The Poe API allows bot developers to invoke other non-premium bots on Poe (including both the bots created by developers and system bots like chatGPT and Claude-Instant) and this access is provided for free so that developers do not have to worry about LLM costs. For every user message, API bot developers get to make up to two calls to another bot of their choice.&#x20;

{% hint style="info" %}
If you are just getting started with API Bots, we recommend checking out our [quick start](quick-start.md) guide. The following tutorial is specifically for how you invoke other bots and assumes that you are familiar with the concept of API Bots.
{% endhint %}

#### Install the poe fastapi client

```bash
pip install fastapi_poe
```

#### Implement the PoeBot class and use the stream\_request function

```python
class BattleBot(PoeBot):
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

## Questions?

Join us on [Discord](https://discord.gg/TKxT6kBpgm) with any questions.
