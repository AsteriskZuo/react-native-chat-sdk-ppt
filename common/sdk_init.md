[return](../main.md)

## Initialize:
在使用SDK之前，一定先进行初始化操作。
初始化可以携带参数。AppKey是必须提供的参数。其它常用参数包括：是否自动登录、是否启动调试模式、是否自动接收好友申请、是否自动接收群组入群邀请、是否已读回执、是否送达回执、是否自动下载等。初始化也是异步处理。通过`then`和`catch`分别返回成功和失败。
Before using the SDK, be sure to initialize it.
Initialization can carry parameters. AppKey is a required parameter. Other common parameters include: whether to log in automatically, whether to start the debugging mode, whether to automatically receive friend requests, whether to automatically receive group invitations, whether to read receipts, whether to deliver receipts, whether to download automatically, etc. Initialization is also handled asynchronously. Return success and failure via `then` and `catch` respectively.

```javascript
// Init sdk.
// Please initialize any interface before calling it.
const init = () => {
  const appKey = "<your app key>";
  let o = new ChatOptions({
    appKey: appKey,
  });
  // execute init operation.
  ChatClient.getInstance()
    .init(o)
    .then(() => {
      // init success
      // todo: something, for example, set listeners.
    })
    .catch((error) => {
      // init fail with error.
    });
};
```

**Note**
使用任何 SDK 接口的之前，请一定进行初始化。
Before using any SDK interface, please be sure to initialize it.
