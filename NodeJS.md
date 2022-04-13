## <font color="red">Node.js</font>

Node.js：ECMAScript + 核心的API（重点）

Node.js是基于Chrome的V8 JavaScript引擎构建的JavaScript运行环境。是一个编译软件。

**特性**：Node.js可以解析JS代码（没有浏览器安全级别的限制）提供很多系统级别的API，如：文件的读写（File System）、进程的管理（Process）、网络通信（HTTP/HTTPS）...

Web服务器 — 一段后台程序

**node.js的内置模块**：<font size="5px">`http、url、queryString、file system、path、global`</font>

### <font color="green">运行js的方式</font>

1、交互模式

> 打开命令行窗口，输入node即进入交互模式，在这个模式下可以写js代码。

2、解释js文件

> 找到文件路径，在地址栏写cmd，在命令窗口写：node 文件路径。

### <font color="green">Nvm — nodejs版本管理器</font>

<font color="red">注意</font>：安装nvm之前需要删除现有的Node。

**nvm命令**

>  nvm version				 	 查看当前nvm版本
>
>  nvm list 							查看当前安装的Node所有版本（常用）
>
>  nvm install 版本号			 安装指定版本的node.js（常用）
>
>  nvm uninstall 版本号		 卸载指定版本的node.js
>
>  nvm use 版本号 			   选择指定版本的node.js（常用）

Nodejs下载地址：https://nodejs.org/en/download/

nvm下载地址：https://github.com/coreybutler/nvm-windows/releases

nvm下载完后在settings.txt文件中添加以下内容可以加快nodejs下载速度

```
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
```

### <font color="green">HTTP模块</font>

​	其他后端语言建立HTTP服务需要借助Apache或nginx的http服务，但Nodejs不需要。

```javascript
//引入http模块
var http = require('http');

// request  获取url（客户端）传过来的信息; response   给浏览器响应信息
http.createServer(function (request, response) {
  // 设置响应头
  response.writeHead(200, {'Content-Type': 'text/html,charset="utf-8"'});  // 解决乱码
  response.write("<head><meta charset='UTF-8'></head>");    // 解决乱码
  // 表示给页面显示一句话并结束响应
  response.end('Hello World');		//没有上两行代码 中文会显示乱码
}).listen(8081);    // 端口：8081

console.log('Server running at http://127.0.0.1:8081/');
```

### <font color="green">url模块</font>

```
url.parse(url);			解析url地址
url.parse(url, true)	把url里的query属性变为 对象
url.parse(url.true).query()
```

### <font color="green">Nodejs自启动工具supervisor</font>

​	supervisor会不停的watch你应用下的所有文件，发现有文件被修改就会重新载入程序文件，修改了程序文件就能马上看到变更后的结果，不用重启nodejs。

1、下载supervisor

```
npm install -g supervisor
// 有时候下载不成功，先去百度搜 cnpm，然后再命令行窗口复制以上命令
```

2、 使用supervisor代替node命令启动应用

```
supervisor 01-basics
```

### <font color="green">commonJs模块和NodeJs模块、自定义模块</font>

​	commonJs的出现是为了弥补Javascript没有**标准库**的缺陷，Nodejs就是commonJs的实现。

​	模块化就是把公共功能抽离成一个js文件成为一个模块，Nodejs里的模块分为**核心模块**和**文件（自定义）模块**，模块里的属性或方法外部不能访问，用的话需要在模块里通过exports或module.exports暴露属性和方法。自定义模块如果在node_modules文件下可以不写完整路径，直接写文件名，否则必须把路径写完整。

```javascript
// 特殊例子
const db = require('db');		// Node默认找 node_modules>db文件下的 index.js
// 需要npm init --yes 强制生成package.json文件
```

### <font color="green">第三方模块</font>

#### 包

​	第三方模块由包组成，可以通过包来对一组具有相互依赖关系的模块进行统一管理，即包由多个模块组成。

​	完全符合CommonJs规范的**包目录**一般包含以下文件：

+ package.json：包描述文件
+ bin：由于存放可执行二进制文件的目录
+ lib：由于存放Javascript代码的目录
+ doc：由于存放文档的目录

<font style="background:#a7a8bd">在NodeJs中可以通过npm命令来下载第三方模块（包）。</font>

#### npm

​	npm是世界上最大的开源代码的生态系统，可以下载各种各样的包，这些包可以在[https://www.npmjs.com/](https://www.npmjs.com/)找到。

**npm命令**

```
npm install moduleName		// 安装模块
npm uninstall moduleName		// 卸载模块
npm list		// 查看当前目录下已安装的包
npm info jquery		// 查看jquery的版本
npm install Name@版本号		// 安装指定版本
npm i 		// 表示如果删掉node_modules文件可以通过该命令找到 package.json 相对应的所有包
npm install name --save		// 下载包并在 package.json里的dependencis里记录下载了哪些包
```

#### package.json

​	package.json定义了这个项目所需要的各种模块，以及项目的配置信息（比如名称、版本、许可证等元数据）。

1、创建package.json

```
npm init --yes
```

2、dependencies与devDependencies之间的区别

​	dependencies			配置当前程序所依赖的其他包

​	devDependencies			配置当前程序所依赖的其他包，比如一些**工具**之类的配置

3、包名后的版本号的**标识符**

> ^表示第一位版本号不变，后两位取最新的
>
> ~表示前两位不变，最后一个取最新
>
> *表示全部取最新

### <font color='green'>fs模块</font>

> fs.stat(path, (err,data)=>{})		检测是文件还是目录
>
> fs.mkdir(path, (err)=>{})			创建目录
>
> fs.writeFile(path, (err)=>{})			创建写入文件
>
> fs.appendFile(path, (err)=>{})			追加文件
>
> fs.readFile(path, (err,data)=>{})			读取文件
>
> fs.readdir(path, (err,data)=>{})				读取目录
>
> fs.rename(path, path, (err)=>{})				重命名/移动文件
>
> fs.rmdir(path, (err)=>{})					删除目录
>
> fs.unlink(path, (err)=>{})					删除文件
>
> 若目录下有文件要 先删除文件 再删除目录

<font style="background:#a7a6bd">注意：nodejs大部份方法都是 异步</font>

