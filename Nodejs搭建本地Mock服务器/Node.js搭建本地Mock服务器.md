# 使用Node.js 搭建Mcok本地服务器

*当我们搭建好移动端（或者前端）界面，需要调试网络请求数据的时候，服务端的同事未必已经准备好接口给我们调用了，这样导致我们的开发过程推延。这时候，搭建一个Mock本地服务器，充当临时的接口调用，是一个不错的选择。*

### 第一步 安装express以及express-generator

```npm install express-generator -g```

### 第二部 创建工程目录
进入你想要创建工程目录的路径

```
express expressDemo

  warning: the default view engine will not be jade in future releases
  warning: use `--view=jade' or `--help' for additional options


   create : expressDemo
   create : expressDemo/package.json
   create : expressDemo/app.js
   create : expressDemo/public
   create : expressDemo/routes
   create : expressDemo/routes/index.js
   create : expressDemo/routes/users.js
   create : expressDemo/views
   create : expressDemo/views/index.jade
   create : expressDemo/views/layout.jade
   create : expressDemo/views/error.jade
   create : expressDemo/bin
   create : expressDemo/bin/www
   create : expressDemo/public/javascripts
   create : expressDemo/public/images
   create : expressDemo/public/stylesheets
   create : expressDemo/public/stylesheets/style.css

   install dependencies:
     $ cd expressDemo && npm install

   run the app:
     $ DEBUG=expressdemo:* npm start
```

接着，进入我们的工程目录，安装依赖包

```
cd expressDemo && npm install
```

安装完成后，运行我们的项目

```
EBUG=expressdemo:* npm start

> expressdemo@0.0.0 start /Users/yancey_chan/Desktop/expressDemo
> node ./bin/www

  expressdemo:server Listening on port 3000 +0ms
```

这时候，打开浏览器，输入http://localhost:3000，正常启动应该看见下图
![](./express_mock.png)

### 第三部 安装mock依赖包
在我们的工程目录下，安装mock依赖包

```
npm install mockjs --save
```

完成后，我们可以看见package.js里面添加我们mockjs的依赖

```
{
  "name": "expressDemo",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "start": "node ./bin/www"
  },
  "dependencies": {
    "body-parser": "~1.17.1",
    "cookie-parser": "~1.4.3",
    "debug": "~2.6.3",
    "express": "~4.15.2",
    "jade": "~1.11.0",
    "mockjs": "^1.0.1-beta3",
    "morgan": "~1.8.1",
    "serve-favicon": "~2.4.2"
  }
}

```

### 第四步 创建mock数据

假设我们的接口地址为  http://host:port/app/test 

其中host和port是你公司服务器的域名和端口号。
/app/test 是接口路径。这时候，我们只需要把除去域名和端口号之后的接口路径写入我们的nodejs路由里面。

在routes文件夹下面，我们添加一个newApp.js文件。加入下面的代码：

```
let Mock = require('mockjs');
let express = require('express');
let router = express.Router();

router.all('/test', (req, res) => {
    res.json(Mock.mock({
        "_RejCode": "000000",
        "List|1-20": [{
            "RandomCWord" : '@cword(5,10)',
            "RandomDate": '@date()',
            "RandomTime": '@time()'
        }]
    }));
})

module.exports = router;
```

再到app.js文件，添加如下代码

```
var newApp = require('./routes/newApp');
app.use('/app', newApp)

```

运行代码，当我们在浏览器输入localhost:3000/app/test时，会看见返回的数据如下：

```
{
	"_RejCode":"000000",
	"List":[
		{
		"RandomCWord":"通加国取层场且",
		"RandomDate":"1997-09-21",
		"RandomTime":"06:56:16"
		},
		{
		"RandomCWord":"术度也适育列因解需",
		"RandomDate":"2015-03-07",
		"RandomTime":"01:41:25"
		},{
		"RandomCWord":"入基会般性又或",
		"RandomDate":"1988-06-11",
		"RandomTime":"09:09:19"
		}
	]
}
```

关于mock的具体使用，这里不过多阐述，有兴趣的可以去mock官网查看具体的语法。

使用本地mock服务器的时候，只要将移动端或许前端的请求重定向到我们的服务器地址即可~例如使用charles等工具可以轻松完成。





