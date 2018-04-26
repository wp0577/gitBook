##### 序言 {#序言}

随着Angular版本的频繁推出，有必要了解下AngularJS、 Angular 2、Angular 4 的区别。

##### 字面上的区别 {#字面上的区别}

（1）我们常说的 Angular 1 是指 AngularJS； 从Angular 2 开始已经改名了。不再带有JS，只是单纯的 Angular；  
（2）还有一个不可思议的版本变化： 从 Angular 2 直接跳跃到了 Angular 4 ， 咋不见 Angular 3 了呢？

##### 架构上的差别 {#架构上的差别}

Angular 1 是一个典型的 MVC 架构 （Model - View - Controller ）， 其架构如图所示：

![](http://upload-images.jianshu.io/upload_images/8940388-2be084ea9d0ab9f8.png?imageMogr2/auto-orient/strip|imageView2/2/w/1240 "MVC 架构示意图 ")

相比于 Angular 1 的MVC 架构， Angular 2 是一个典型的基于组件（component\) 架构。从这一点上来说，它与 React.js 结构相似。如下图所示：  
![](http://upload-images.jianshu.io/upload_images/8940388-c59f497233f8b577.png?imageMogr2/auto-orient/strip|imageView2/2/w/1240 "Angular 2  架构")

##### 为何匆忙推出 Angular 2 ？ {#为何匆忙推出-angular-2}

照理说，Angular 1. x 版本已经足够强大，为什么还匆匆忙忙推出 Angular 2 呢？这是迫于 mobile apps 的需要。按照惯性的思维： Angular 2 应该是 Angular 1.x 的升级版本，其实不然， Angular 2 与 Angular 1.x 完全不同， 最基本的语法都不一样。 Angular 1.x 是 基于 JavaScript的框架，而Angular 2 是基于 TypeScript的框架。

所以说，当你决定要学习 Angular 时，要想好是学 Angular 1 还是 Angular 2。 那么到底学哪个版本好呢？ 这不好讲，还得看项目需要。 如果单纯地学习，当然是越高的版本越好，与时俱进嘛！

##### Angular 3 怎么不见了？ {#angular-3-怎么不见了}

Angular 团队开发 Angular 3时，在router模块上出现了问题， 再三纠结，决定放弃 Angular 3 ，直接奔向了 Angular 4

##### Angular 2 有什么好？ {#angular-2-有什么好}

相比 Angular 1.x， Angular 2 的体积更小，为什么这么做，说白了，一个字——`快`； 如果仅仅用于PC 端的WEB开发， Angular 1.x足以应对； 如果是用于 mobile app ，在用户体验方面，略显捉襟见肘！

##### Angular 4 有什么好？ {#angular-4-有什么好}

Angular 4 是 Angular 2 的升级版本， 也就是说，从 Angular 2之后，它们的版本一脉相承，是升级版本，而不是推到重来的版本。 Angular 4 比 Angular 2 更快。  
所以说， 从 Angular 1.x 到 Angular 2 ，再发展到 Angular 4， 其路线就是为了更快一些。

##### Angular 1写的代码无法用在Angular 2上，这是为什么？ {#angular-1写的代码无法用在angular-2上这是为什么}

Angular 1 代码是基于 JavaScript 写的， 代码示例：

```
var
 angular1 = angular.module('uiroute', ['ui.router']);
 angular1.controller('CarController', function($scope)
 {
    $scope.CarList = ['Audi', 'BMW', 
'Bugatti', 'Jaguar'];
});
```

---

Angular 2 代码 是基于 TypeScript 写的。 TypeScript与JavaScript 的区别大了去了。 TypeScript 是 JavaScript的超集 （superset）。 看一段 Angular 2 代码：

    import { platformBrowserDynamic } from 
    "@angular/platform-browser-dynamic"
    ;
    import { AppModule } from 
    "./app.module"
    ;
    platformBrowserDynamic().bootstrapModule(AppModule);  
    import { NgModule } from 
    "@angular/core"
    ;  
    import { BrowserModule } from 
    "@angular/platform-browser"
    ;  
    import { AppComponent } from 
    "../app/app.component"
    ;  
    @NgModule({  
        imports: [BrowserModule],  
        declarations: [AppComponent],  
        bootstrap: [AppComponent]  
    })  
    export 
    class
     AppModule { }  
    import { Component } from 
    '@angular/core'

    @Component({  
        selector: 
    'app-loader'
    ,  
        template: `  
    <div>
    <div><h4>
    Welcome to Angular with ASP.NET Core and Visual Studio 2017
    <
    /
    h4
    >
    <
    /
    div
    >
    <
    /
    div
    >

    `  
    })  
    export class AppComponent{}

如果不熟悉 TypeScript 语法，上面这段代码不知所云！ 既然差异这么大，把 Angular 1 升级到 Angular 2 难度之大，可以预见！

这么看来， Angular 1 与 2 的差别，并不是什么框架上的差别，而是它们的语法完全不一样， 一个用JavaScript，一个用 TypeScript。 那为什么Angular 4 是 Angular 2 的升级版呢？ 答案很简单， 因为 4和2 用的都是 TypeScript 用法！

##### 代码重用方法 {#代码重用方法}

在 Angular 1 中，最为常用的是`$scope`在 Angular 2和4中被去掉了。在新版本中，更多推崇的是`directive`和`controller`, 通过对`component`组件的split（分割），从而实现代码的复用。

##### 对 Mobile app 的支持 {#对-mobile-app-的支持}

Angular 1的设计初衷是为了实现响应式网页、双向数据绑定的Web应用，如果从Html5的概念来看，Angular 1 算是一个很好的支持H5的前端框架了。 如果我们对Angular 有更高的期望，那就是希望Angular 能很好地支持 mobile app，达到APP 原生的用户体验效果。 而这正是 Angular 1 的短板，鉴于词，才推出了 Angular 2 及其后来的Angular 4。

接下来，我们重点谈谈 Angular 2 的架构

##### Angular 2 架构 {#angular-2-架构}

可以说， Angular 2 是面向 mobile app 的架构，为了达到APP 原生的效果， Angular 2 特有引入了 NativeScript 技术。

![](http://upload-images.jianshu.io/upload_images/8940388-a2c739eb82017bd3.png?imageMogr2/auto-orient/strip|imageView2/2/w/1240 "Angular 2 的APP 效果")

##### 如何解决APP 跨平台问题 {#如何解决app-跨平台问题}

Angular 2 解决了 mobile app 跨平台的问题， 所谓跨平台是指，用 Angular 2 编写的 Web 在 iOS 和 Android 上能达到同等原生的用户体验效果，只需要编写一套代码。

#### 小结 {#小结}

如果你是刚接触 Angular 开发，建议从 Angular 2 开始，相对要简单些； 何况 Angular 4 还在持续更新中，带版本稳定后，再向 Angular 4 进发！

