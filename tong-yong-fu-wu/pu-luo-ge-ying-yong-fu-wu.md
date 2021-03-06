## 普罗格应用服务-PrologApplicationService

> ### 简介

普罗格应用服务提供了打印服务、称重服务功能。

* 打印服务是一个兼容多种打印方案的通用服务，支持B/S、C/S打印，支持客户端部署和集中部署。服务使用web api接口对外提供服务。打印服务是基于grid++report开发，所以报表模板和数据grid++report规范。
* 称重服务是通过串口与电子秤进行连接，读取到数据后以websocket server方式对外发布数据，因此与客户端通过websocket协议进行连接。

> ### 安装

普罗格应用服务目前只支持windows系统，且系统支持.netframework4.5。

安装步骤

1、下载安装程序，当前版本为1.0.7，点击安装即可。

2、安装grid++类库支持

安装完成后界面如下，打印服务默认端口6060，服务接口[http://localhost:6060/print。称重服务默认端口8887](http://localhost:6060/print。称重服务默认端口8887)

![](/assets/import8172.png)

![](/assets/import8173.png)

> ### 快速使用

### 打印服务快速使用

1、设置参数  
单击“服务设置”按钮，进入设置界面，如下图示。在设置界面中，可对端口、默认打印机、超时时间、工作路径及打印方案进行设置。  
    打印方案和打印机是多对一的关系。

![](/assets/import8175.png)

2、启动服务  
单击主界面中“启动服务”按钮即可启动服务，左侧的状态框中会显示打印服务的启停状态。

3、提交打印请求  
客户端调用方式  
客户端直接使用ajax请求进行打印请求，默认提交路径：[http://localhost:6060/print，支持GET和POST请求。](http://localhost:6060/print，支持GET和POST请求。)  
请求参数

```json
 {"url": "",
    "printName":"",
    "printSolution":"",
    "data":[]
 }

 参数说明
 url -（必填） 模版，可以为url路径，也可以为json数据，当为模版路径时，在主界面上选择模版类型为“URL”，当为模版json数据时，选择模版类型为“data”

 printName -（选填） 打印机名称，选择相同名称的打印机进行打印，当打印方案“printSolution”参数为空时，参数生效。

 printSolution - （选填）使用打印方案对应的打印机进行打印。

 data - （必填）打印数据，可以为URL地址，也可以为json或xml数据。数据类型和数据格式需要进行对应的设置。

 备注
 printName参数和printSolution参数都为空时，或者选择了锁定打印机时，服务会使用默认打印机打印
```

4、打印或预览  
在主界面打印设置中，选择“打印”，将发送打印机进行打印，当选择“预览”将进行预览

![](/assets/import8179.png)

5、示例

```js
//通过ajax提交请求
 $.ajax({
 type: "POST",
 url: "http://localhost:6060/print", //打印服务地址，print是固定值
 data: {
    "url": "http://www.gridreport.cn/demos/grf/4d.grf",
    "printName":"Microsoft XPS Document Writer",
    "printSolution":"solution1",
    "data":["http://www.gridreport.cn/demos/data/DataCenter.ashx?data=SubReport_4d&city=%E5%A4%A9%E6%B4%A5"]
 }, //报表数据 
 contentType:'application/json',
 dataType: "json",
 success: function(data){
             //处理返回值
             //{"success":true,"message":"打印请求成功"}
          }
});
```

### 称重服务快速使用

1、设置参数

![](/assets/import8173.png)

2、启动服务

单击“启动服务”按钮启动服务

3、客户端程序示例

```
// 初始化一个 WebSocket 对象
var ws = new WebSocket("ws://localhost:8887");

// 建立 web socket 连接成功触发事件
ws.onopen = function () {
  // 使用 send() 方法发送数据
  ws.send("发送数据");
  alert("数据发送中...");
};

// 接收服务端数据时触发事件
ws.onmessage = function (evt) {
  var received_msg = evt.data;
  alert("数据已接收...");
};

// 断开 web socket 连接成功触发事件
ws.onclose = function () {
  alert("连接已关闭...");
};
```



