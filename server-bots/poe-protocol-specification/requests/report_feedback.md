# report\_feedback

This request takes the following additional parameters:

* `message_id` (identifier with type m): The message for which feedback is provided.
* `user_id` (identifier with type `u`): The user providing the feedback
* `conversation_id` (identifier with type `c`): The conversation giving rise to the feedback
* `feedback_type` (string): May be `like` or `dislike`. Additional types may be added in the future; bot servers should ignore feedback with types they do not recognize.

### Response

The server’s response is ignored.
