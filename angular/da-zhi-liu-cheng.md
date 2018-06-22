# 大致流程

Angular 大致有三个主要的元素和其他元素若干。

Component，module，router。

Component是组成整个网站框架的组件。

![](../.gitbook/assets/import%20%2845%29.png)

app.component.ts的功能就是页面展示，可以调用外部html，并且可以有很多子组件。

```text
import { Component } from '@angular/core';
import { UserComponent } from "../user/user.component";

interface Address {
  province: string;
  city: string;
}

@Component({
  selector: 'app-root',
  template: `<app-user></app-user>`
    ,
})
export class AppComponent {}
```

app.module.ts是根模块，它可以import进其他所有其他的module，整个angular的启动就是从app.module.ts启动。

module将component间进行关联。

```text
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from "@angular/forms";

import { AppComponent } from './app.component';
import { UserComponent } from '../user/user.component';

@NgModule({
  declarations: [
    AppComponent,
    UserComponent
  ],
  imports: [
    FormsModule,
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

