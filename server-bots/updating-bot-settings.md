# Updating bot settings

The settings endpoint provides a way for you to opt in/out of Poe's features enabling you to customize the behavior of the bot. This article will describe how you can get Poe to fetch the latest settings from your bot.

#### 1. Set up your endpoint as described by the specs

If you are using the `fastapi_poe` library, then you just need to implement the `get_settings` method in the `PoeBot` class. The following is an example:

```python
async def get_settings(self, setting: SettingsRequest) -> SettingsResponse:
    return SettingsResponse(allow_attachments=True)
```

#### 2. Get your access key

You can find this key by going to the bot page and clicking the gear icon.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

#### 3. Make a post request to Poe's refetch settings endpoint with your bot name and access key.

On Windows, you can use the `Invoke-RestMethod` command. On a Macbook or Linux machine, you can use the curl command as follows:

`curl -X POST https://api.poe.com/bot/fetch_settings/<botname/<access_key>`

That's it. The response to the above request will inform you whether the updated successfully.
