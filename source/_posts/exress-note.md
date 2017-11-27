# express托管静态文件

1. express.static

- Express内置的express.static中间件可以方便托管静态文件
`app.use(express.static('public'));`</br>
现在可以访问public目录下的文件了
`http://localhost:3000/images/kitten.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/images/bg.png
http://localhost:3000/hello.html`

- 可以多次调用，同时托管多个文件夹

2. 建立虚拟路径来访问

- `app.use('/static', express.static('public'));`</br>
可以通过带有 “/static” 前缀的地址来访问 public 目录下面的文件了。
`http://localhost:3000/static/images/kitten.jpg
http://localhost:3000/static/css/style.css
http://localhost:3000/static/js/app.js
http://localhost:3000/static/images/bg.png
http://localhost:3000/static/hello.html`

# 404处理
404在express不是捕获的错误，而是express执行了所有中间件和路由之后还是没有获取任何输出，所以我们需要对这个做一下处理</br>
`app.use(function(req, res, next) {
  res.status(404).send('Sorry cant find that!');
});`

# 错误处理器
错误处理器中间件的定义和其他中间件一样，唯一的区别是 4 个而不是 3 个参数，即 (err, req, res, next)</br>
`app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});`

# 路由
1. `app.METHOD(path, [callback...], callback)`</br>
    `var express = require('express');
    var app = express();
    app.get('/', function(req, res) {
      res.send('hello world');
    });`</br>
    路由是由一个 URI、HTTP 请求（GET、POST等）和若干个句柄组成