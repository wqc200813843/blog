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
- 路由方法
    
    `app.METHOD(path, [callback...], callback)`</br>
    ```
    var express = require('express');
    var app = express();
    // GET method route
    app.get('/', function (req, res) {
      res.send('GET request to the homepage');
    });
    // POST method route
    app.post('/', function (req, res) {
      res.send('POST request to the homepage');
    });
    ```
    路由是由一个 URI、HTTP 请求（GET、POST等）和若干个句柄组成

- 捕获不用类型的请求

```
  app.all('/secret', function (req, res, next) {
  console.log('Accessing the secret section ...');
  next(); // pass control to the next handler
  });
```
- 路由路径

    1. ?
        
        代表前一个字符存在或者不存在都匹配</br>
        ab?cd匹配acd,abcd
    2. +

        匹配前一个字符一个或者多个<br>
        ab+cd匹配abcd,abbcd,abbbcd
    3. *

        代表*位置可以替换为任何字符串<br>
        ab*cd匹配abcd,abxcd,abRABDOMcd,ab123cd 
    4. ()

        将括号内的东西统一处理<br>
        ab(cd)?e匹配abe,abcde
    5. /a/

        /a/匹配任何路径中含有a的路径
- 路由句柄
  
    多个回调函数处理路由
    ```
    var cb0 = function (req, res, next) {
      console.log('CB0');
      next();
    }
    var cb1 = function (req, res, next) {
      console.log('CB1');
      next();
    }
    app.get('/example/d', [cb0, cb1], function (req, res, next) {
      console.log('response will be sent by the next function ...');
      next();
    }, function (req, res) {
      res.send('Hello from D!');
    });
    ```
    可以同时用函数和函数数组，前一个函数通过next()跳转到下一个函数
- app.route()
    ```
    app.route('/book')
    .get(function(req, res) {
      res.send('Get a random book');
    })
    .post(function(req, res) {
      res.send('Add a book');
    })
    .put(function(req, res) {
      res.send('Update the book');
    });
    ```
    链式路由句柄，可以监听统一路由多种类型的请求

- express.Router

    创建模块化，可挂载的路由句柄

    路由文件
    ```
    var express = require('express');
    var router = express.Router();
    // 该路由使用的中间件
    router.use(function timeLog(req, res, next) {
      console.log('Time: ', Date.now());
      next();
    });
    // 定义网站主页的路由
    router.get('/', function(req, res) {
      res.send('Birds home page');
    });
    // 定义 about 页面的路由
    router.get('/about', function(req, res) {
      res.send('About birds');
    });
    module.exports = router;
    ```
    加载路由模块的应用
    ```
    var birds = require('./birds');
    ...
    app.use('/birds', birds);
    ```
    应用即可处理发自 /birds 和 /birds/about 的请求
# 中间件
- 应用级中间件

    1. app.use() 和 app.METHOD()
    2. 多次调用
        ```
        // 一个中间件栈，处理指向 /user/:id 的 GET 请求
        app.get('/user/:id', function (req, res, next) {
          console.log('ID:', req.params.id);
          next();
        }, function (req, res, next) {
          res.send('User Info');
        });

        // 处理 /user/:id， 打印出用户 id
        app.get('/user/:id', function (req, res, next) {
          res.end(req.params.id);
        });
        ```
        
        因为第一个路由的res.send已经终止了请求-响应循环，所以第二个路由永远不会触发

    3. 跳到下一个路由 
        next('route'),只对app.VERB和router.VERB有效
- 路由级中间件

    express.Router

- 错误处理中间件

    ```
    app.use(function(err, req, res, next) {
      console.error(err.stack);
      res.status(500).send('Something broke!');
    });
    ```
    要4个参数，不然不能处理错误
- 内置中间件

    4.x版本开始，express内部只有express.static这个中间件，用来挂载静态资源
    ```
    express.static(root, [options])
    ```
    root指提供静态资源的根目录
    
- 第三方中间件

    1. 安装所需功能的node模块 `npm install xxx`
    2. `app.use`