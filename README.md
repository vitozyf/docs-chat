## 交易中心

### 通信示例

#### 父页面=》子页面

- 父页面

```javascript
this.$refs.Trading.contentWindow.postMessage(
  JSON.stringify({
    token: 'some token'
  }),
  '*'
);
```

- 子页面

```javascript
methods: {
    receiveMessageFromIndex(event) {
      try {
        const DATA = JSON.parse(event.data);
        if (DATA.token) {
            // do some thing
        }
      } catch (error) {
         // error
      }
    }
},
mounted() {
    window.addEventListener('message', this.receiveMessageFromIndex, false);
},
```

#### 子页面=》父页面

- 子页面

```javascript
parent.postMessage(JSON.stringify({ test: 'aaaaa' }), '*');
```

- 父页面

```javascript
methods: {
    readyHandler(e) {
      try {
        const DATA = JSON.parse(event.data);
        // do some thing
      } catch (error) {
        // error
      }
    }
},
mounted() {
    on(window, 'message', this.readyHandler);
},
```

### 路由

- 侧边栏 `http://192.168.1.10:8023/#/TradingChat`
- 批量询价 `http://192.168.1.10:8023/#/Upload`

### 接受数据

| 字段           | 值                                                                                                       |
| -------------- | -------------------------------------------------------------------------------------------------------- |
| token          | 用户 token String                                                                                        |
| offerData      | 记录报价信息 Object                                                                                      |
| inquiryDatas   | 询价勾选的数据信息 Array                                                                                 |
| chatType       | 切换侧边栏状态（ 0-聊天消息列表， 1-批量询价 2-聊天窗口 3-记录报价）                                     |
| chatTypeParams | 切换侧边栏状态的参数，和状态同时修改，可为空（外部切换到聊天窗口时需要填该参数{CompanyName, CompanyID}） |

### 发出数据

| 字段           | 值                                                                                                                   |
| -------------- | -------------------------------------------------------------------------------------------------------------------- |
| datas          | Array<{Model, Brand, Qty}>                                                                                           |
| size           | 页面尺寸{height}                                                                                                     |
| inquiryResults | 询价结果 boolean                                                                                                     |
| -------------- | ------------------------------                                                                                       |
| openStatus     | 聊天打开状态（触发时只有关闭，即 false）                                                                             |
| loginInfo      | {Code, Message} (登录异常信息不再内部提示，目前抛出的状态包括： Code: 0 => 登录正常， Code: 401 => 身份过期、未登录) |
