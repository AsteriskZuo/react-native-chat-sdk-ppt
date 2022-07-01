[return](../main.md)

## Registration steps:
用户需要提供用户名和密码两个参数。通过REST请求执行注册操作。当执行成功会通过`then`返回，如果失败会通过`catch`返回并携带错误提示。
The user needs to provide two parameters: username and password. Perform registration operations via REST http requests. When the execution is successful, it will return through `then`, if it fails, it will return through `catch` with an error message.

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

  // Request to register an account.
  const requestRegistryAccount = () => {
    // Request the specified address.
    return requestHttp('https://a1.easemob.com/app/chat/user/register');
  };

  const registerAccount = () => {
    // Register a user using the REST interface.
    requestRegistryAccount()
      .then(response => {
        // register success.
      })
      .catch(error => {
        // register fail with error.
      });
  };
```
