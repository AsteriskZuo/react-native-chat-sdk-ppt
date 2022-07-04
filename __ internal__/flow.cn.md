# 演示剧本

## 开发环境要求

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

[中文]

- xcode 使用 app store 安装。
- android studio 从官网下载最新的稳定版安装。
  - 安装完成需要更新或者下载一些依赖，项目打开它会自动下载。下载超时可能需要重试
- nodejs 从官方下载最新稳定版，或者使用 brew 进行便捷安装。
  - 安装之后，自带 npm 管理工具。
  - 使用 brew 安装: `brew install node@16`
- yarn 通过 npm 进行全局安装。
  - 使用 npm 命令安装：`npm install -g yarn`
- watchman 工具使用 brew 进行安装。
  - 使用 brew 安装: `brew install watchman`
- visual studio code 从官网下载最新版本进行安装。

## 创建项目

[中文]

创建项目使用 npm 命令: `npx react-native init SomeProject`。

## 添加 SDK 插件

```sh
yarn add agora-react-native-chat
```

# 对于 ios 平台

```sh
cd ios && pod install
```

1. Run app debug server `npx react-native start --port 8888`.
2. Open `SomeProject` with xcode.
3. Select simulator for x86_64 architecture.
4. Click `start the active scheme` button, build and run app project.

# 对于 android 平台

```sh
open -a "/Applications/Android Studio.app"
```

1. Run app debug server `npx react-native start --port 8888`.
2. Open `SomeProject` with android studio.
   **note**: This version of the Android Support plugin for IntelliJ IDEA (or Android Studio) cannot open this project that 0.69 version, please retry with version 2021.1.1 or newer.
3. Select simulator for x86_64 architecture.
4. Click `Run app` button, build and run app project.

## Implement source code

```javascript

```

## Refresh UI

## 测试

1. 执行初始化操作
2. 执行注册操作
3. 执行登录操作
4. 执行发送操作
5. 退出登录

6. 执行初始化操作
7. 执行注册操作
8. 执行登录操作
9. 等待对方发送消息，并接收消息
10. 退出登录
