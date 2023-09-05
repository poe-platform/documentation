# Quick start

In this quick start, we will build a backend bot server and then integrate it with Poe. Once you have created a Poe bot powered by your backend, any Poe user can interact with your bot. The following diagram might be useful in visualizing how your bot server fits into Poe.

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
For more information on Poe server bots, check out the [poe-protocol-specification](poe-protocol-specification/ "mention").
{% endhint %}

## Deploying your bot

We recommend using [Modal](https://modal.com/?utm\_source=poe) to deploy your bot but you can also use any cloud provider of your choice; all you need to do is to make the bot server available at a publicly available URL and once you have that, you can skip to [integrating it with Poe](quick-start.md#integrating-with-poe). In order to use Modal to deploy your bot, do the following.

#### Step 1: Install the Modal client

Open a terminal and run `pip install modal-client`

{% hint style="info" %}
If you are using a MacBook with Apple silicon and encounter errors about incompatible architecture when installing the modal-client, try running `pip install modal-client --compile --force-reinstall` instead.
{% endhint %}

#### Step 2: Setup your Modal token

This step involves setting up access to modal from your terminal. You only need to do this once for your computer. In the terminal, run `modal token new --source poe`. You will taken to your web browser where you will be asked to log into modal using your Github account.

<figure><img src="../.gitbook/assets/login.png" alt=""><figcaption></figcaption></figure>

After you login, click on "create token". You will be prompted to close the browser window after that.

<figure><img src="../.gitbook/assets/create_token.png" alt=""><figcaption></figcaption></figure>

#### Step 3: Clone the starter code and deploy to Modal

In your terminal, run:

* `git clone https://github.com/poe-platform/server-bot-quick-start`
* `cd server-bot-quick-start`
* `pip install -r requirements.txt`
* `modal deploy main.py`

Modal will now deploy your app and output two urls: a) the endpoint at which your app is hosted b) an internal page where you can monitor your app. You will be using the former to integrate your bot into Poe.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

## Integrating with Poe

Once you have a bot running under a publicly accessible URL, it is time to connect it to Poe. You can do that on your desktop by going to the bot creation [form](https://poe.com/create\_bot?server=1). You can customize how your bot looks by providing a picture, name and description. After you fill out the server URL and click "create bot", your bot should be ready for use on all Poe clients.

## Iterating on your bot

For faster iteration on your bot, Modal offers the serve command which you can use by running `modal serve main.py`. Modal will deploy an ephemeral app on the cloud which gets live-updated when you make changes to the underlying code. Feel free to comment/uncomment any of the other example bots to try them out or build off of them.

## Where to go from here

* Refer to the [specification](poe-protocol-specification/) to understand the full capabilities offered by Poe server bots
* Check out the [fastapi-poe](https://pypi.org/project/fastapi-poe/) library, which you can use as a base for creating Poe bots
