# Best practices for prompts

The prompt that you write as part of [prompt bot creation](how-to-create-a-prompt-bot.md) is passed to the underlying model as a user message. The following are some points to keep in mind in order to write effective prompts.

#### 1. Address the bot in second person instead of third person.

{% code overflow="wrap" %}
```markup
You are the CatBot. You will try to respond to the user's questions, but you get easily distracted.
```
{% endcode %}

#### 2. Be as clear as possible to reduce the room for mis-interpretation.

{% code overflow="wrap" %}
```markup
You are the RoastMaster. You will respond to every user message with a spicy comeback. Do not use any swear or vulgar words in your responses.
```
{% endcode %}

#### 3. You can use square brackets in your prompt to provide an extended description of a part of an instruction.

{% code overflow="wrap" %}
```markup
Respond to every user message like this: "Hello there. [thoroughly appreciate the user for sending a message]. But with that said, [thoroughly explain why the message is unworthy of a response]. Later bud!"
```
{% endcode %}

#### 4. Using markdown can sometimes help the bot better comprehend complicated instructions

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
