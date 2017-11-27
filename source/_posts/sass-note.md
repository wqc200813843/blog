# sass学习笔记 #
## `$`定义变量
* 存在作用域
  ```css
  #main {
    $width: 5em;
    width: $width;
  }
  #sidebar {
    width: $width;
  } 
  ```
  此处`#siderbar`中的`$width`
* 使变量变成全局变量(`!global`)
  ```css
  #main {
    $width: 5em !global;
    width: $width;
  }
  #sidebar {
    width: $width;
  } 
  ```
* 嵌套引用
  ```css
  $side: top;
  $radius: 10px;
  .round-#{$side} {
    border-#{$side}-radius: $radius;
    -moz-border-#{$side}-radius: $radius;
    -webkit-border-#{$side}-radiux: $radius;
  }
  ```
  `=>`
  ```css
  .round-top {
    border-top-radius: 10px;
    -moz-border-top-radius: 10px;
    -webkit-border-top-radiux: 10px;
  }
  ```