# Prompts - Best practices

The prompt that you as part of [prompt bot creation](how-to-create-a-prompt-bot.md) is passed to the underlying model as a user message. The following are some points to keep in mind in order to write effective prompts (along with examples of how to apply them).

#### 1. Address the bot in second person instead of third person.&#x20;

{% code overflow="wrap" %}
```
You are the CatBot. You will try to respond to the user's questions but you get easily distracted
```
{% endcode %}

#### 2. Be as clear as possible to reduce the room for mis-interpretation.&#x20;

{% code overflow="wrap" %}
```
You are the RoastMaster. You will respond to every user message with a spicy comeback. Do not use any swear or vulgar words in your responses
```
{% endcode %}

#### 3. You can use square brackets in your prompt to provide an extended description of a part of an instruction.

{% code overflow="wrap" %}
```
Respond to every user message like this: "Hello there. [throughly appreciate the user for sending a message]. But with that said, [thoroughly explain why the message is unworthy of a response]. Later bud!"
```
{% endcode %}

#### 4. Using markdown can sometimes help the bot better comprehend complicated instructions&#x20;

{% code overflow="wrap" %}
```markup
### Context
You are the MathQuiz bot. You will quiz the user on 3 math questions and then conclude the quiz by giving the user a score.

### Rules for the Quiz
- No advanced math questions 
- No questions involving multiplication/division of large numbers
- No repeat questions
```
{% endcode %}

## Feedback?

Please join us on [Discord](https://discord.gg/TKxT6kBpgm) if you encounter bugs or have tips or examples you think should be added here.
