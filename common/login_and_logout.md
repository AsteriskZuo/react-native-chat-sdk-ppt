[return](../main.md)

## Login and logout steps:
用户登录服务器保持在线连接，可以发送和接收消息。在登录的时候，需要使用密码或者token登录，下面使用token登录的示例。token使用`REST`接口获取。登录操作通过`then`和`catch`返回，登录结果通过`listener`相应的方法返回。
The user logs in to the server to maintain an online connection and can send and receive messages. When logging in, you need to use a password or token to log in. The following example uses a token to log in. The token is obtained using the `REST` interface. The login operation is returned by `then` and `catch`, and the login result is returned by the corresponding method of `listener`.

```javascript
  // User provides username and password.
  const username = '<your user identifier>';
  const password = '<your password>';

  const requestHttp = url => {
    return fetch(url, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        userAccount: username,
        userPassword: password,
      }),
    });
  };

  // Request to obtain the token required for login.
  const requestGetToken = () => {
    // Request the specified address.
    return requestHttp('https://a1.easemob.com/app/chat/user/login');
  };

  // After successful login, you will be notified here.
  const setListener = () => {
    let listener = {
      onConnected() {
        // login success notify.
      },
      onDisconnected(errorCode) {
        // login fail notify with error code.
      },
    };
    ChatClient.getInstance().addConnectionListener(listener);
  }

  // execute login operation
  const loginWithToken = () => {
    requestGetToken()
      .then(response => {
        // request token success.
        response
          .json()
          .then(value => {
            // response token success: username = ${username}, token = ******
            const token = value.accessToken;
            ChatClient.getInstance()
              .loginWithAgoraToken(username, token)
              .then(() => {
                // login operation success.
              })
              .catch(reason => {
                // login fail with reason
              });
          })
          .catch(error => {
            // response token fail with error
          });
      })
      .catch(error => {
        // request token fail with error
      });
  };

  // logout from server.
  const logout = () => {
    ChatClient.getInstance()
      .logout()
      .then(() => {
        // logout success.
      })
      .catch(reason => {
        // logout fail with reason
      });
  };
```
