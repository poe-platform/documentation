# How to create a prompt bot

## Step 1: Open the create bot page

If you are using a web browser, go to [poe.com](https://poe.com) and click on the "Create a bot" button in the left sidebar. Alternatively you can navigate directly to [poe.com/create\_bot](https://poe.com/create\_bot). If you are using the Poe mobile app, tap the ≡ icon the top-left to open the sidebar and then tap on the "Create a bot" button at the top of the menu.

## Step 2: Customize how your bot will look

#### Upload a picture (Optional)

This helps give your bot a distinctive appearance.

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt="" width="254"><figcaption></figcaption></figure>

#### Select a bot name

The name is a unique identifier and can be 4-15 alphanumeric characters. The field will come prefilled with a randomly generated name that you can leave as it is. You can always come back and update the name later.

<figure><img src="../.gitbook/assets/image (9).png" alt="" width="563"><figcaption></figcaption></figure>

#### Write a bot description (Optional)

A piece of text that describes your bot. This will be shown to people who land on the bot page.

<figure><img src="../.gitbook/assets/image (4).png" alt="" width="563"><figcaption></figcaption></figure>

## Step 3: Define your bot's behavior

#### Select a base bot

This is the underlying bot that your prompt bot is powered by.

<figure><img src="../.gitbook/assets/image (6).png" alt="" width="563"><figcaption></figcaption></figure>

#### Provide a prompt

Describe what your bot should do or how it should behave. For more information on how to write a good prompt, check out [prompts best practices](best-practices-for-prompts/) guide. You can also use the "Prompt visible from bot profile" option to decide whether your prompt should be visible to the public.

<figure><img src="../.gitbook/assets/image (7).png" alt="" width="563"><figcaption></figcaption></figure>

#### Provide a knowledge base (Optional)

For text generation bots, you can add a knowledge base to provide external information for your bot to reference. Your bot will retrieve relevant parts of the knowledge base for a given user message and use them to inform its response.

A knowledge base consists of one or more knowledge sources which are created from uploaded files or raw text input. Major text file formats (such as .txt, .pdf, .docx, and .html) are supported for file uploads. The size of each knowledge base is limited to 5GB or 30 million characters combined across all knowledge sources.

You can optionally enable "Cite sources" for your bot. This will enable citation UI elements for your bot in addition to instructing the bot to produce in-line citations when appropriate.

<figure><img src="../.gitbook/assets/image.png" alt="" width="563"><figcaption></figcaption></figure>

#### Provide an intro message (Optional)

This is the message that the bot starts with when users land on the bot page. This can be used to explain to the users what the bot does or get necessary information that the bot needs to get started.

<figure><img src="../.gitbook/assets/image (10).png" alt="" width="563"><figcaption></figcaption></figure>

## Step 4: Decide if you need any advanced features (Optional)

#### Suggest replies

Whether a user should see some LLM-generated reply options after getting a response from the bot. This could help in lowering friction for users if the option is a good fit for your bot.

#### Render markdown content

Allows messages with the bot to be rendered using Markdown. If this is off, messages are rendered as plain text.&#x20;

Poe supports GitHub-Flavored Markdown (GFM, specified at [https://github.github.com/gfm/](https://github.github.com/gfm/)). However, for security and privacy reasons, prompt bots only support images hosted at imgur.com and unsplash.com. Images that use any other URL will fail to render. Please reach out to us on [Discord](https://discord.gg/TKxT6kBpgm) if you wish to make a request for us to support additional image hosting sites.

#### Custom temperature

Allows you to specify the temperature you want to use for your bot. Higher temperatures will introduce more randomness in your bot's responses. Lower temperatures will produce more consistent results, which can be useful for fact-based bots (especially those equipped with a knowledge base).

## Step 5: Create your bot

After filling out all the required fields, click on the "Create bot" button at the bottom of the screen. That's it! You will be taken to your new bot's page, and from there you can start chatting or share the bot with your friends.
