# 项目部署

## Node Server
Node.JS的服务转发接口项目

### 源码
https://github.com/thingsroot/nodeServer.git

### 手工启动

1. 进入源码目录
2. 运行 ``` npm install ``` 安装依赖包
3. 运行 ``` npm start ``` 开启服务

### 服务启动(Windows)

1. 进入源码目录
2. 运行 ``` npm install ``` 安装依赖包
3. 运行 ``` npm install node-windows --save ``` 安装node-windows模块
4. 创建nw.js文件， 文件内容为:

``` js
let Service = require('node-windows').Service;
let svc = new Service({
  name: 'node_test',    //服务名称
  description: '测试项目服务器', //描述
  script: '<源码目录>/bin/www.js'  //nodejs项目要启动的文件路径
});

svc.on('install', () => {
  svc.start();
});
```
5. 执行node nw.js进行后台服务注册


### 服务启动(Linux)
使用Supervisor启动.
``` ini
[program:react-node-server]
command=/usr/bin/node <code_path>/bin/www.js
priority=4
autostart=true
autorestart=true
stdout_logfile=<log_path>/node-server.log
stderr_logfile=<log_path>/node-server.error.log
user=frappe
directory=<code_path>
```


### 服务端口

NodeServer默认端口为8881。 在bin/www.js(文件末尾)中可以修改端口号

### Frappe 后台地址

NodeServer默认使用ioe.thingsroot.com的Frappe后台服务接口。 可在bin/www.js中修改path地址(第六行)


## React Project

### 源码
https://github.com/thingsroot/ReactProject.git

### 本地启动

1. 进入源码目录
2. 运行 ``` npm install ``` 安装依赖包
3. 运行 ``` npm run start ``` 启动本地服务
4. 访问: http://localhost:3000

#### Node Server 服务端口

如果启动Node Server时候改用了其他端口，则需要手工修改 config/webpackDevServer.config.js 中对应的端口

### 服务器部署

1. 进入源码目录
2. 运行 ``` npm install ``` 安装依赖包
3. 运行 ``` npm run build ``` 打包文件
4. 进入build 子目录复制所有文件到nginx的网站文件夹
5. 建立转发规则: ```/api/<path> => <node_server>:<node_server_port>/<path>```
