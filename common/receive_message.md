[return](../main.md)

## Receive message steps:
接收消息只需要设置消息接收监听器。一般地，在登录成功之后马上设置。防止漏掉消息。
To receive a message, you only need to set the message receiving listener. Generally, it is set immediately after a successful login. Avoid missing messages.

```javascript
// set message receive listener
let msgListener = {
  onMessagesReceived(messages) {
    // todo: receive one or more message
  },
};

// register message listener
ChatClient.getInstance().chatManager.addMessageListener(msgListener);
```
