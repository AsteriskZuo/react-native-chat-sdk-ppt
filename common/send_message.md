[return](../main.md)

## Send message steps:
这里是发送消息示例。我们先创建一个文本消息，发送给某人，设置消息回调来接收发送消息的结果。
Here is an example of sending a message. We start by creating a text message, sending it to someone, and setting up a message callback to receive the result of sending the message.

```javascript
// send text message to somebody
const sendmsg = () => {
  const userId = '<message receive target identifier>';
  const content = '<message text content>';

  // create text message
  let msg = ChatMessage.createTextMessage(
    userId,
    content,
    ChatMessageChatType.PeerChat
  );

  // set send message result
  const callback = new (class {
    onProgress(locaMsgId, progress) {
      // send message process: ${locaMsgId}, ${progress}.
    }
    onError(locaMsgId, error) {
      // send message fail with error.
    }
    onSuccess(message) {
      //send message success.
    }
  })();

  // execute send message operation
  ChatClient.getInstance()
    .chatManager.sendMessage(msg, callback)
    .then(() => {
      // send message operation success.
    })
    .catch((reason) => {
      // send message fail with reason.
    });
};
```
