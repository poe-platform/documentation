# How we cover your costs

Our intent is to cover all model inference costs and any other significant per-message costs involved in operating any bot on Poe. There are two ways this can happen at present:

* If you use the [bot query API](accessing-other-bots-on-poe.md), then the cost of any model inference used by any bot you query will be automatically covered by Poe.
  * This is the easiest option and should be used whenever possible. If you are using a standard base model that is available via a bot on Poe and there is a reason this option does not work for your use case, please [get in contact with us](../resources/how-to-contact-us.md).
* If you want to use a model that is not currently available on Poe (and therefore not available through the bot query API), we can work with you to pay your model inference costs. Depending on how expensive they are, we may need to restrict availability to your bot to Poe subscribers only, or to limit the number of messages per day that users can send to it.
  * If this is a fine-tuned version of a standard open source model, this will generally involve hosting the model on an inference provider like Fireworks, Together, or Replicate, and giving us visibility into the cost per token or per item.
  * If you are training your own model from scratch and hosting your own inference, we can discuss how we can best cover inference costs.
  * Please [contact us](../resources/how-to-contact-us.md) for more information about either of the above options.
* If there is some other especially expensive input to your bot (e.g. use of a web search API), we can likely cover that as well.
