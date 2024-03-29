# Concepts

### Identifiers

The protocol uses identifiers for certain request fields. These are labeled as “identifier” in the specification. Identifiers are globally unique. They consist of a sequence of 1 to 3 lowercase ASCII characters, followed by a hyphen, followed by 32 lowercase alphanumeric ASCII characters or `=` characters (i.e., they fulfill the regex `^[a-z]{1,3}-[a-z0-9=]{32}$`).

The characters before the hyphen are a tag that represents the type of the object. The following types are currently in use:

* `m`: represents a message
* `u`: represents a user
* `c`: represents a conversation
* `d`: represents metadata sent with a message

### Authentication

While creating a bot, a creator can provide an access key consisting of 32 ASCII characters. To allow bot servers to confirm that the requests come from Poe, all requests will have an Authorization HTTP header “Bearer \<access\_key>”.

### Content types

Messages may use the following content types:

* `text/plain`: Plain text, rendered without further processing
* `text/markdown`: Markdown text. Specifically, this supports all features of GitHub-Flavored Markdown (GFM, specified at [https://github.github.com/gfm/](https://github.github.com/gfm/)). Poe may however modify the rendered Markdown for security or usability reasons.

### Versioning

It is expected that the API will be extended in the future to support additional features. The protocol version string consists of two numbers (e.g., 1.0).

The first number is the _request version_. It will be incremented if the form the request takes changes in an incompatible fashion. For example, the current protocol only contains a single API call from Poe servers to bot servers. If we were to add another API call that bot servers are expected to support, we would increment the request version (e.g., from 1.0 to 2.0). This is expected to be very rare, and it will be communicated to bot servers well in advance.

The second number is the _response version._ It will be incremented whenever the Poe server adds support for a new feature that bot servers can use. For example, as of this version we support two content types in LLM responses. If we were to add a third, we would increment the response version (e.g., from 1.0 to 1.1). Bot servers written for an earlier response version will continue to work; they simply will not use the new feature.

The response version is also incremented for backward-compatible changes to the request. For example, if we add a new field to the request body, we would increment the response version. This is safe for old bot servers as they will simply ignore the added field.

Throughout the protocol, bot servers should ignore any dictionary keys without a specified meaning. They may be used in a future version of the protocol.

### Limits

Poe may implement limits on bot servers to ensure the reliability and scalability of the product. In particular:

* The initial response to any request must be returned within 5 seconds.
* The response to any request (including `query` requests) must be completed within 120 seconds.
* The total length of a bot response (the sum of the length of all `text` events sent in response to a `query` request) may not exceed 100,000 characters.
* The total number of events sent in response to a `query` event may not exceed 10,000.
* If the number of messages in a user's previous conversation with the bot exceeds 1000, Poe may truncate the conversation.

We may raise these limits in the future if good use cases come up.
