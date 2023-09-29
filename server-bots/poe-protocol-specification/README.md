# Poe Protocol Specification

[Poe](https://poe.com/) is a platform for interacting with AI-based bots. Poe provides access to popular chat bots like OpenAI's GPT-3.5-Turbo and Anthropic's Claude, but also allows a developer to create their own bot by implementing the following protocol.

## Introduction

This specification provides a way to run custom bots from any web-accessible service that can be used by the Poe app. Poe users will send requests to the Poe server, which will in turn send requests to the bot server using this specification. As the bot server responds, Poe will show the response to the user.

{% hint style="info" %}
See [Quick start](../quick-start.md) for a high-level introduction to running a server bot.
{% endhint %}

### Terminology

* _**Poe server**:_ server run by Poe that receives client requests, turns them into requests to bot servers, and streams the response back to the Poe client.
* _**Bot server**:_ server run by the developer that respond to requests from Poe servers. The responses are ultimately shown to users in their Poe client.

