[return](../main.md)

## Set offline message receive:
这个内容可能有点多，具体是详见`https://rnfirebase.io`。  
客户端方面需要提供`send id`和`device token`。  
服务器的控制台需要提供`send id`和`FCM Token`。  
对于`iOS`平台，FCM的控制台需要上传`apns certificate`，下载`GoogleService-Info.plist`文件并集成到App中，以及项目的相关配置。  
对于`Android`平台，FCM的控制台需要下载`google-services.json`文件并集成到App中，以及项目的相关配置。  
下面是用户需要实现的代码示例。       

This content may be a bit too much, see `https://rnfirebase.io` for details.
The client side needs to provide `send id` and `device token`.
The console of the server needs to provide `send id` and `FCM Token`.
For the `iOS` platform, the FCM console needs to upload the `apns certificate`, download the `GoogleService-Info.plist` file and integrate it into the App, as well as the related configuration of the project.
For the `Android` platform, the FCM console needs to download the `google-services.json` file and integrate it into the App, as well as the related configuration of the project.
Below is the code sample that the user needs to implement.

```javascript
const appKey = "<your app key>";
const deviceId = "<fcm send id when you registered.>";
const fcmToken = "<current device token from callback>";
let pushConfig = new ChatPushConfig({
  deviceId: deviceId,
  deviceToken: fcmToken,
});
let o = new ChatOptions({
  appKey: appKey,
  pushConfig: pushConfig,
});

// Pass the id and token to the server.
const sendPushConfig = () => {
  const deviceId = "<fcm send id when you registered.>";
  const fcmToken = "<current device token from callback>";

  ChatClient.getInstance()
    .updatePushConfig(new ChatPushConfig({ deviceId, deviceToken }))
    .then(() => {
      // update push parameters success.
    })
    .catch((reason: any) => {
      // update push parameters fail with reason.
    });
};
```

**Note**
对于 iOS 平台，必须在初始化的时候进行设置才生效。
For the iOS platform, it must be set during initialization to take effect.
