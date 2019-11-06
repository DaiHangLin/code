# <center>qt quick 简介</center>

https://note.youdao.com/web/#/file/recent/markdown/WEBca586918e0e7726ebe7e9330769b8676/

## 背景
> 为了解决electron在创建多个相对独立的不规则窗体的不足，比如： 
> 1. 圆形窗体效果不够满意，比如拖拽，点击穿透等
> 2. 实现类似popup窗体的代价大
> 3. 透明窗体

## 什么是qt quick

1. qt是一个跨平台应用程序和UI开发框架，某种意义上和electron的存在为了同样的一个使命。
2. qt quick是其中的一种高级用户界面技术，说白了就是标准图形库。
3. QML 的 JavaScript 不是浏览器中的 JavaScript，也不是 nodejs 中的 JavaScript，因为 QML 中的 JavaScript 的宿主环境不同于浏览器和 nodejs，所以 QML 中的 JavaScript 不会有诸如 window，dom 这些浏览器才会预设的全局变量。

## 优点

1. 快速发开动画式的用户界面
2. 语法类似CSS，通俗易懂。
3. 声明式代码结构，清晰明了。
~~~css
Rectangle {
  id: rect
  width: 400
  height: width + 50
  color: "red"
}
~~~

## helloworld
>  主要介绍windows下简单操作

1. 安装
   
   - 先安装visual studio 2017/2015
   - 官网下载qt creator，安装时只需要选择相关的windows模块就可以了
2. 编辑器介绍  

![image](https://note.youdao.com/yws/res/1159/WEBRESOURCEf8684025865dd4ffe8d41e58aa9741b8)

    - 可以看到qt creator提供了丰富的示例和教程，上手比较容易。
    - 创建项目
    
![image](https://note.youdao.com/yws/res/1165/WEBRESOURCE0b54a60306dda89b9a62458ddf63348e)

    - 一路next就ok了
    - 直接运行
![image](https://note.youdao.com/yws/res/1172/WEBRESOURCE79cd89b2f95d92324bbde88c550d1a91)

    - helloworld已经运行完毕了


## 简单分析

- 如何加载 

把qml作为一个QObject和C++进行关联，由Qt管理QML的上下文。


- 数据绑定


```
Rectangle {
    id:rect
    width:10 // 绑定到一个数字
    height:10 + 10 //绑定到表达式
    radius:{
        if(width > 10) return 2;
        else return 3;
    } // 绑定到一个代码块
    color: getColor()

   function getColor() {
      if (true) {
      return "transparent"
      } else {
      return "blue"
      }
   } // 绑定到一个函数
}
```

## 解决需求
完成一个简单的圆形可拖拽，不显示在任务栏中的窗体 

![image](https://note.youdao.com/yws/res/1192/WEBRESOURCEb596f3084a6cc7b8c8e93585f9763254)
> 只需要简单设置windows的flag就可以完成了，

> Qt.Tool 设置在任务栏不显示logo

> Qt.WindowStaysOnTopHint 永远在最上层

#### 代码结构
>  Window  // 定义一个全屏的window，无边框，隐藏任务栏，透明背景
>    
>    Rectangle // 居中，红色，圆形
>       
>       MouseArea // 修改window的x，拖拽效果
>       
>       state // press事件
>
下面的代码基本都是上面声明式代码的补充，可以看到，qml的编写界面的逻辑是相当简单的，而且不到40行代码就解决了之前electron在写这个单独的窗体带来的痛

![image](https://note.youdao.com/yws/res/1245/WEBRESOURCEf7ed72ede91366d12c17150bb9d63fe4)


## deploy


```
 C:\Qt\5.13.1\msvc2017\bin\windeployqt.exe -qmldir=C:\Users\namek\work\ray-toolbars .\ray-toolbars.exe 
```
