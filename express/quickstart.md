# 快速入门
## 安装
首先假定你已经安装了 Node.js，接下来为你的应用创建一个目录，然后进入此目录并将其作为当前工作目录。
```
$ mkdir myapp
$ cd myapp
```
通过 npm init 命令为你的应用创建一个 package.json 文件。
```
$ npm init
```
此命令将要求你输入几个参数，例如此应用的名称和版本。 你可以直接按“回车”键接受大部分默认设置即可，下面这个除外：
```
entry point: (index.js)
```
键入 app.js 或者你所希望的名称，这是当前应用的入口文件。如果你希望采用默认的 index.js 文件名，只需按“回车”键即可。
接下来在 myapp 目录下安装 Express 并将其保存到依赖列表中。如下：
```
$ npm install express --save
```
如果只是临时安装 Express，不想将它添加到依赖列表中，可执行如下命令：
```
$ npm install express --no-save
```
## HelloWorld
```
const express = require('express');
const app = express();

app.get('/', (req, res) => res.send('Hello World!'));

app.listen(3000, () => console.log('Example app listening on port 3000!'));
```
运行：
```
node app.js
```
## Express 应用程序生成器
通过应用生成器工具 express-generator 可以快速创建一个应用的骨架。
express-generator 包含了 express 命令行工具。通过如下命令即可安装：
```
$ npm install express-generator -g
```
-h 参数可以列出所有可用的命令行参数：
```
$ express -h

  Usage: express [options] [dir]

  Options:

    -h, --help          输出使用方法
        --version       输出版本号
    -e, --ejs           添加对 ejs 模板引擎的支持
        --hbs           添加对 handlebars 模板引擎的支持
        --pug           添加对 pug 模板引擎的支持
    -H, --hogan         添加对 hogan.js 模板引擎的支持
        --no-view       创建不带视图引擎的项目
    -v, --view <engine> 添加对视图引擎（view） <engine> 的支持 (ejs|hbs|hjs|jade|pug|twig|vash) （默认是 jade 模板引擎）
    -c, --css <engine>  添加样式表引擎 <engine> 的支持 (less|stylus|compass|sass) （默认是普通的 css 文件）
        --git           添加 .gitignore
    -f, --force         强制在非空目录下创建
```
例如，如下命令创建了一个名称为 myapp 的 Express 应用。此应用将在当前目录下的 myapp 目录中创建，并且设置为使用 Pug 模板引擎（view engine）：
```
$ express --view=pug myapp

   create : myapp
   create : myapp/package.json
   create : myapp/app.js
   create : myapp/public
   create : myapp/public/javascripts
   create : myapp/public/images
   create : myapp/routes
   create : myapp/routes/index.js
   create : myapp/routes/users.js
   create : myapp/public/stylesheets
   create : myapp/public/stylesheets/style.css
   create : myapp/views
   create : myapp/views/index.pug
   create : myapp/views/layout.pug
   create : myapp/views/error.pug
   create : myapp/bin
   create : myapp/bin/www
```
然后安装所有依赖包：
```
$ cd myapp
$ npm install
```
启动应用：
```
//linux
$ DEBUG=myapp:* npm start
//windows
set DEBUG=myapp:* & npm start
```
然后在浏览器中打开 http://localhost:3000/ 网址就可以看到这个应用了。
通过生成器创建的应用一般都有如下目录结构：
```
.
├── app.js
├── bin
│   └── www
├── package.json
├── public
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
├── routes
│   ├── index.js
│   └── users.js
└── views
    ├── error.pug
    ├── index.pug
    └── layout.pug

7 directories, 9 files
```

## 路由
路由的定义遵循以下结构
```
app.METHOD(PATH, HANDLER)
```
其中：
- **app** is an instance of express.
- **METHOD** is an HTTP request method, in lowercase.
- **PATH** is a path on the server.
- **HANDLER** is the function executed when the route is matched.

例如：
```
app.get('/', function (req, res) {
  res.send('Hello World!')
})

app.post('/', function (req, res) {
  res.send('Got a POST request')
})

app.put('/user', function (req, res) {
  res.send('Got a PUT request at /user')
})

app.delete('/user', function (req, res) {
  res.send('Got a DELETE request at /user')
})
```

## 静态文件
为了提供诸如图像、CSS 文件和 JavaScript 文件之类的静态文件，请使用 Express 中的 express.static 内置中间件函数。
此函数特征如下：
```
express.static(root, [options])
```

例如，通过如下代码就可以将 public 目录下的图片、CSS 文件、JavaScript 文件对外开放访问了：
```
app.use(express.static('public'))
```
现在，你就可以访问 public 目录中的所有文件了：
```
http://localhost:3000/images/kitten.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/images/bg.png
http://localhost:3000/hello.html
```
> Note:Express 在静态目录查找文件，因此，存放静态文件的目录名不会出现在 URL 中。

如果要使用多个静态资源目录，请多次调用 express.static 中间件函数：
```
app.use(express.static('public'))
app.use(express.static('files'))
```
访问静态资源文件时，express.static 中间件函数会根据目录的添加顺序查找所需的文件。

还可以为静态文件提供前缀目录 如：
```
app.use('/static', express.static('public'))
```
现在，你就可以通过带有 /static 前缀地址来访问 public 目录中的文件了。
```
http://localhost:3000/static/images/kitten.jpg
http://localhost:3000/static/css/style.css
http://localhost:3000/static/js/app.js
http://localhost:3000/static/images/bg.png
http://localhost:3000/static/hello.html
```
如果要访问其他目录下的静态文件，使用绝对路径更安全！
```
app.use('/static', express.static(path.join(__dirname, 'public')))
```
