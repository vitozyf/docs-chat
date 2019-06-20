## 交易中心

### 说明

- BOM 上传，交易中心聊天的 iframe, 发布在`BOMAI主站`的`Trading`目录下

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

| 字段           | 值                                                                                                                             |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| token          | 用户 token `string`                                                                                                            |
| offerData      | 记录报价信息 `object`                                                                                                          |
| inquiryModel   | 询价型号 `string`                                                                                                              |
| inquiryDatas   | 询价勾选的数据信息 `array`                                                                                                     |
| chatType       | 切换侧边栏状态（ 0-聊天消息列表， 1-批量询价 2-聊天窗口 3-记录报价） `number`                                                  |
| chatTypeParams | 切换侧边栏状态的参数，和状态同时修改，可为空（外部切换到聊天窗口时需要填该参数`{CompanyName, CompanyID, TradingInquiryData}`） |

- `chatTypeParams`
  - `CompanyName` <string>
  - `CompanyID` <string>
  - `TradingInquiryData` ?<{Model, Brand}> (可选参数， 直接询价窗口)

### 发出数据

| 字段           | 值                                                                                                                     |
| -------------- | ---------------------------------------------------------------------------------------------------------------------- |
| datas          | `Array<{Model, Brand, Qty}>`                                                                                           |
| size           | 页面尺寸 `{height}`                                                                                                    |
| inquiryResults | 询价结果 `boolean`                                                                                                     |
| -------------- | ------------------------------                                                                                         |
| openStatus     | 聊天打开状态（`false`关闭，`true`打开） `boolean`                                                                      |
| loginInfo      | `{Code, Message}` (登录异常信息不再内部提示，目前抛出的状态包括： Code: 0 => 登录正常， Code: 401 => 身份过期、未登录) |
| unReadNumbers  | 未读消息条数 `number`                                                                                                  |
| inquiryResult  | 询价结果 `null/Array<>`                                                                                                |
