# Welcome to Poe for Developers

Poe is a platform for people to discover and talk to AI-powered bots. In addition to providing access to popular bots, Poe allows anyone to create new bots.

### Reasons to create a bot on Poe

* **Costs**: Operating a publicly available bot powered by large language models is prohibitively expensive for most, but creating a bot on Poe allows you to [offload model inference costs entirely to Poe](server-bots/how-we-cover-your-costs.md).
* **Distribution**: Poe's bot recommendation system allows you to reach a large audience for your bot for free. Users can also conveniently share a chat with your bot both internally on Poe and externally, leading to even more exposure for your bot.
* **Multi-platform UI**: Poe's native presence on all major platforms (Web, iOS, Android, MacOS, etc) means that your users have a great, consistent experience with your bot no matter what device they are on, with login and synchronized history taken care of by the platform.
* **Model independence**: Building on Poe lets you build your product using models from all different providers (OpenAI, Anthropic, Google, and open source models like Llama 2 or SDXL). Poe is a neutral platform and so by building here, you can continually adapt your product use any combination of the best technologies as they are created, no matter who each one is made by.

In summary, Poe lets developers focus entirely on the unique part of their bot: how it will respond. The platform handles everything else needed to bring it to a large audience and to operate it at scale, both now and as AI progress continues.

### Ways to create a bot on Poe

We provide two different developer products and depending on your needs and use case, one or the other might be a better fit for you.

* **Prompt bots** are bots built on top of other bots (for example Claude-instant, GPT-4, or Llama 2) using plain-text instructions that the bot will stick to during conversation with users. To learn how to build a prompt bot, check out our [prompt bot tutorial](prompt-bots/how-to-create-a-prompt-bot.md).
* **Server bots** are bots powered by a custom backend that runs your code in response to every user message. In addition, building a server bot with Poe allows you to call any other bot (like GPT-3.5-Turbo, Claude-instant, or any prompt bot) on Poe for free, which can significantly enhance the capabilities of your bot without you having to incur a large cost. To learn how to build a server bot, check out our [quick start](server-bots/quick-start.md).
