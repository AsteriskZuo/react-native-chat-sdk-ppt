# Demonstration script

## Development environment requirements

MacOS 10.15 or above
Xcode 12.4 or above for iOS mobile device
Android studio 4.2 or above for Android mobile device
NodeJs between 16 and 18
React Native between 0.66.4 and 0.69, but exclude 0.69
React between 17 and 18, but exclude 18
npm tools，NPM is installed with NodeJS.
yarn tools，It is installed using NPM.
watchman tools, It is install using brew.
vscode latest version

[English]

- It is recommended to maintain the latest stable version of the operating system.
- xcode is installed using the app store.
- android studio download the latest stable version from the official website and install it.
  - After the installation is complete, you need to update or download some dependencies. It will be downloaded automatically when the project is opened. Download timeout may require retry
- Download the latest stable version of nodejs from the official website, or use brew for easy installation.
  - After installation, it comes with npm management tool.
  - Install with brew: `brew install node@16`
- yarn is installed globally via npm.
  - Install using npm command: `npm install -g yarn`
- The watchman tool is installed using brew.
  - Install with brew: `brew install watchman`
- download the latest version of visual studio code from the official website and install it.

```sh
sw_vers
xcodebuild -version
brew --version
node --version
npm --version
yarn --version
watchman --version
open -a "/Applications/Android Studio.app"
```

## Creation project

[English]

Create a project using the npm command: `npx react-native init SomeProject`.
Create a project with specific version using the npm command: `npx react-native init SomeProject --version 0.68.2`.

## Add SDK plug -in

```sh
yarn add agora-react-native-chat
```

# for ios platform

```sh
cd ios && pod install
```

1. Run app debug server `npx react-native start --port 8888`.
2. Open `SomeProject` with xcode.
3. Select simulator for x86_64 architecture.
4. Click `start the active scheme` button, build and run app project.

# for android platform

```sh
open -a "/Applications/Android Studio.app"
```

1. Run app debug server `npx react-native start --port 8888`.
2. Open `SomeProject` with android studio.
   **note**: This version of the Android Support plugin for IntelliJ IDEA (or Android Studio) cannot open this project that 0.69 version, please retry with version 2021.1.1 or newer.
3. Select simulator for x86_64 architecture.
4. Click `Run app` button, build and run app project.

## Implement DEMO source code

```javascript
// import depend packages.
import React, { useEffect } from "react";
import {
  SafeAreaView,
  ScrollView,
  StyleSheet,
  Text,
  TextInput,
  View,
} from "react-native";
import {
  ChatClient,
  ChatOptions,
  ChatMessageChatType,
  ChatMessage,
} from "agora-react-native-chat";

// The App Object.
const App = () => {
  // variable defines.
  const title = "AgoraChatQuickstart";
  const [appKey, setAppKey] = React.useState("81446724#514456");
  const [username, setUsername] = React.useState("");
  const [password, setPassword] = React.useState("");
  const [userId, setUserId] = React.useState("");
  const [content, setContent] = React.useState("");
  const [logText, setWarnText] = React.useState("Show log area");

  // output console log.
  useEffect(() => {
    logText.split("\n").forEach((value, index, array) => {
      if (index === 0) {
        console.log(value);
      }
    });
  }, [logText]);

  // output ui log.
  const rollLog = (text) => {
    setWarnText((preLogText) => {
      let newLogText = text;
      preLogText
        .split("\n")
        .filter((value, index, array) => {
          if (index > 8) {
            return false;
          }
          return true;
        })
        .forEach((value, index, array) => {
          newLogText += "\n" + value;
        });
      return newLogText;
    });
  };

  const requestHttp = (url) => {
    return fetch(url, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        userAccount: username,
        userPassword: password,
      }),
    });
  };
  const requestGetToken = () => {
    return requestHttp("https://a1.easemob.com/app/chat/user/login");
  };
  const requestRegistryAccount = () => {
    return requestHttp("https://a1.easemob.com/app/chat/user/register");
  };

  // register listener for message.
  const setMessageListener = () => {
    let msgListener = {
      onMessagesReceived(messages) {
        for (let index = 0; index < messages.length; index++) {
          rollLog("received msgId: " + messages[index].msgId);
        }
      },
      onCmdMessagesReceived: (messages) => {},
      onMessagesRead: (messages) => {},
      onGroupMessageRead: (groupMessageAcks) => {},
      onMessagesDelivered: (messages) => {},
      onMessagesRecalled: (messages) => {},
      onConversationsUpdate: () => {},
      onConversationRead: (from, to) => {},
    };

    ChatClient.getInstance().chatManager.removeAllMessageListener();
    ChatClient.getInstance().chatManager.addMessageListener(msgListener);
  };

  // Init sdk.
  // Please initialize any interface before calling it.
  const init = () => {
    let o = new ChatOptions({
      autoLogin: false,
      appKey: appKey,
    });
    ChatClient.getInstance().removeAllConnectionListener();
    ChatClient.getInstance()
      .init(o)
      .then(() => {
        rollLog("init success");
        this.isInitialized = true;
        let listener = {
          onTokenWillExpire() {
            rollLog("token expire.");
          },
          onTokenDidExpire() {
            rollLog("token did expire");
          },
          onConnected() {
            rollLog("login success.");
            setMessageListener();
          },
          onDisconnected(errorCode) {
            rollLog("login fail: " + errorCode);
          },
        };
        ChatClient.getInstance().addConnectionListener(listener);
      })
      .catch((error) => {
        rollLog(
          "init fail: " +
            (error instanceof Object ? JSON.stringify(error) : error)
        );
      });
  };

  // register account for login
  const registerAccount = () => {
    if (this.isInitialized === false || this.isInitialized === undefined) {
      rollLog("Perform initialization first.");
      return;
    }
    rollLog("start register account ...");
    requestRegistryAccount()
      .then((response) => {
        rollLog(`register success: userName = ${username}, password = ******`);
      })
      .catch((error) => {
        rollLog("register fail: " + JSON.stringify(error));
      });
  };

  // login with account id and token
  const loginWithToken = () => {
    if (this.isInitialized === false || this.isInitialized === undefined) {
      rollLog("Perform initialization first.");
      return;
    }
    rollLog("start request token ...");
    requestGetToken()
      .then((response) => {
        rollLog("request token success.");
        response
          .json()
          .then((value) => {
            rollLog(
              `response token success: username = ${username}, token = ******`
            );
            const token = value.accessToken;
            rollLog("start login ...");
            ChatClient.getInstance()
              .loginWithAgoraToken(username, token)
              .then(() => {
                rollLog("login operation success.");
              })
              .catch((reason) => {
                rollLog("login fail: " + JSON.stringify(reason));
              });
          })
          .catch((error) => {
            rollLog("response token fail:" + JSON.stringify(error));
          });
      })
      .catch((error) => {
        rollLog("request token fail: " + JSON.stringify(error));
      });
  };

  // logout from server.
  const logout = () => {
    if (this.isInitialized === false || this.isInitialized === undefined) {
      rollLog("Perform initialization first.");
      return;
    }
    rollLog("start logout ...");
    ChatClient.getInstance()
      .logout()
      .then(() => {
        rollLog("logout success.");
      })
      .catch((reason) => {
        rollLog("logout fail:" + JSON.stringify(reason));
      });
  };

  // send text message to somebody
  const sendmsg = () => {
    if (this.isInitialized === false || this.isInitialized === undefined) {
      rollLog("Perform initialization first.");
      return;
    }
    let msg = ChatMessage.createTextMessage(
      userId,
      content,
      ChatMessageChatType.PeerChat
    );
    const callback = new (class {
      onProgress(locaMsgId, progress) {
        rollLog(`send message process: ${locaMsgId}, ${progress}`);
      }
      onError(locaMsgId, error) {
        rollLog(`send message fail: ${locaMsgId}, ${JSON.stringify(error)}`);
      }
      onSuccess(message) {
        rollLog("send message success: " + message.localMsgId);
      }
    })();
    rollLog("start send message ...");
    ChatClient.getInstance()
      .chatManager.sendMessage(msg, callback)
      .then(() => {
        rollLog("send message: " + msg.localMsgId);
      })
      .catch((reason) => {
        rollLog("send fail: " + JSON.stringify(reason));
      });
  };

  // ui render.
  return (
    <SafeAreaView>
      <View style={styles.titleContainer}>
        <Text style={styles.title}>{title}</Text>
      </View>
      <ScrollView>
        <View style={styles.inputCon}>
          <TextInput
            multiline
            style={styles.inputBox}
            placeholder="Enter appkey"
            onChangeText={(text) => setAppKey(text)}
            value={appKey}
          />
        </View>
        <View style={styles.buttonCon}>
          <Text style={styles.btn2} onPress={init}>
            INIT SDK
          </Text>
        </View>
        <View style={styles.inputCon}>
          <TextInput
            multiline
            style={styles.inputBox}
            placeholder="Enter username"
            onChangeText={(text) => setUsername(text)}
            value={username}
          />
        </View>
        <View style={styles.inputCon}>
          <TextInput
            multiline
            style={styles.inputBox}
            placeholder="Enter password"
            onChangeText={(text) => setPassword(text)}
            value={password}
          />
        </View>
        <View style={styles.buttonCon}>
          <Text style={styles.eachBtn} onPress={registerAccount}>
            SIGN UP
          </Text>
          <Text style={styles.eachBtn} onPress={loginWithToken}>
            SIGN IN
          </Text>
          <Text style={styles.eachBtn} onPress={logout}>
            SIGN OUT
          </Text>
        </View>
        <View style={styles.inputCon}>
          <TextInput
            multiline
            style={styles.inputBox}
            placeholder="Enter the username you want to send"
            onChangeText={(text) => setUserId(text)}
            value={userId}
          />
        </View>
        <View style={styles.inputCon}>
          <TextInput
            multiline
            style={styles.inputBox}
            placeholder="Enter content"
            onChangeText={(text) => setContent(text)}
            value={content}
          />
        </View>
        <View style={styles.buttonCon}>
          <Text style={styles.btn2} onPress={sendmsg}>
            SEND TEXT
          </Text>
        </View>
        <View>
          <Text style={styles.logText} multiline={true}>
            {logText}
          </Text>
        </View>
        <View>
          <Text style={styles.logText}>{}</Text>
        </View>
        <View>
          <Text style={styles.logText}>{}</Text>
        </View>
      </ScrollView>
    </SafeAreaView>
  );
};

// ui styles sets.
const styles = StyleSheet.create({
  titleContainer: {
    height: 60,
    backgroundColor: "#6200ED",
  },
  title: {
    lineHeight: 60,
    paddingLeft: 15,
    color: "#fff",
    fontSize: 20,
    fontWeight: "700",
  },
  inputCon: {
    marginLeft: "5%",
    width: "90%",
    height: 60,
    paddingBottom: 6,
    borderBottomWidth: 1,
    borderBottomColor: "#ccc",
  },
  inputBox: {
    marginTop: 15,
    width: "100%",
    fontSize: 14,
    fontWeight: "bold",
  },
  buttonCon: {
    marginLeft: "2%",
    width: "96%",
    flexDirection: "row",
    marginTop: 20,
    height: 26,
    justifyContent: "space-around",
    alignItems: "center",
  },
  eachBtn: {
    height: 40,
    width: "28%",
    lineHeight: 40,
    textAlign: "center",
    color: "#fff",
    fontSize: 16,
    backgroundColor: "#6200ED",
    borderRadius: 5,
  },
  btn2: {
    height: 40,
    width: "45%",
    lineHeight: 40,
    textAlign: "center",
    color: "#fff",
    fontSize: 16,
    backgroundColor: "#6200ED",
    borderRadius: 5,
  },
  logText: {
    padding: 10,
    marginTop: 10,
    color: "#ccc",
    fontSize: 14,
    lineHeight: 20,
  },
});

export default App;
```

## Update UI

## Run and test

First device:

1. Execute initialization operation
2. Execute the registration operation
3. Execute the login operation
4. Execute the sending operation
5. Exit login

Second device:

6. Execute initialization operation
7. Registration operation
8. Execute the login operation
9. Waiting for the other party to send a message and receive the message
10. Exit login
