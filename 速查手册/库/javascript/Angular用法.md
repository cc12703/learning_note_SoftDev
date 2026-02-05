


# Angular用啊

[TOC]

## 概述


### 特点
* 使用`组件`来构建应用
* 使用`信号`来管理状态，实现响应性
* 使用`模版`来创建动态界面
* 使用`依赖注入`来进行模块化



## 基础语法

### 信号
* `<name> = signal(<default>)` 创建
* `<name>.set(<val>)`          设置值
* `<name>.update(<val> => ...)` 更新值

* `<name> = computed(() => ...)` 创建计算信号
    * 特点：只读，会被缓存，惰性求值

* `effect(() => { ... })`  创建副作用
    * 特点：会在信号被更改时被调用
* `effect((onCleanup) => { ... })`  带清理函数


#### 示例
```ts

effect((onCleanup) => {
  const user = currentUser();

  const timer = setTimeout(() => {
    console.log(`1 second ago, the user became ${user}`);
  }, 1000);

  onCleanup(() => {    //注册清理函数
    clearTimeout(timer);
  });

});
```

### 指令


## 模版语法

### 属性绑定
* `{{ <val> }}`        文本
* `[<prop>] = "..."`   对象属性
    * 绑定到：DOM对象、组件实例、指令实例上
* `[attr.<name>] = "..."`  HTML属性
* `[class] = "..."`         CSS类
* `[style.<name>] = "..."`  CSS属性


#### 示例
```ts
@Component({
  template: `
    <p>{{ welcomeMessage }}</p> 
    <p>Your color preference is {{ theme() }}.</p> 

    <ul [attr.role]="listRole()">
  `
})

export class AppComponent {
  welcomeMessage = "Welcome, enjoy this app that we built for you"; 
  theme = signal('dark');
}
```


### 事件绑定
* `(<event>) = "..."`  监听原生事件
    * `$event`：提供事件信息

#### 示例
```ts
@Component({
  template: `
    <input type="text" (keyup)="updateField($event)" />
  `
})
export class AppComponent{
  updateField(event: KeyboardEvent): void {
    console.log('Field is updated!');
  }
}
```

### 双向绑定
* 一种简写形式，结合了属性绑定和事件绑定
* `[(<name>)] = "..."`


#### 示例
```ts
// 和表单控件进行绑定
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';

@Component({
  imports: [FormsModule],
  template: `
    <main>
      <h2>Hello {{ firstName }}!</h2>
      <input type="text" [(ngModel)]="firstName" />
    </main>
  `
})
export class AppComponent {
  firstName = 'Ada';
}
```

### 控制流
* `@if | @else-if | @else` 条件显示
* `@for(<item> of <set>; track <item-id>)`  循环显示
    * `track`: 维护数据和DOM之间的关联
    * `$count`：条目的数量
    * `$index`：当前的索引
* `@empty`  集合为空
* `@switch | @case | @default` 条件显示

#### 示例
```ts

@if (a > b) {
  {{a}} is greater than {{b}}
} @else-if (b > a) {
  {{a}} is less than {{b}}
} @else {
  {{a}} is equal to {{b}}
}

@for (item of items; track item.id) {
  {{ item.name }}
}

@for (item of items; track item.id; let idx = $index, e = $even) {
  <p>Item #{{ idx }}: {{ item.name }}</p>
}

@switch (userPermissions) {
  @case ('admin') {
    <app-admin-dashboard />
  }
  @case ('reviewer') {
    <app-reviewer-dashboard />
  }
  @case ('editor') {
    <app-editor-dashboard />
  }
  @default {
    <app-viewer-dashboard />
  }
}
```

## 组件语法

### 构成
* TS类：包含行为
* HTML模版：渲染界面
* CSS选择器：定义如何使用组件

#### 示例
```ts
@Component({
  selector: 'profile-photo',
  template: `<img src="profile-photo.jpg" alt="Your profile photo">`,
  styles: `img { border-radius: 50%; }`,
})
export class ProfilePhoto { }


@Component({
  selector: 'profile-photo',
  templateUrl: 'profile-photo.html',    //使用独立文件
  styleUrl: 'profile-photo.css',        //使用独立文件
})
export class ProfilePhoto { }
```

### 选择器
* `<name>`       类型选择器
* `[<name>]`     属性选择器
* `[<key>="<val>"]` 属性选择器
* `.<name>`      类选择器
* `<sel>,<sel>`  多个选择器

#### 示例
```ts
@Component({
  selector: 'drop-zone, [dropzone]',
})

@Component({
  selector: 'button[type="reset"]',  //组合
})

@Component({
  selector: '[dropzone]:not(textarea)', //不匹配textarea元素
})
```

### 生命周期
* `constructor`: 构造函数，组件被实例化

* `ngOnInit`: 初始化完成，输入值已被设置
* `ngAfterContentInit`: 子组件都初始化完成
* `ngAfterContentInit`: 模版中子条目都初始化完成

* `ngOnDestroy`: 被销毁前

* `ngOnChanges`: 输入发生变更

### 输入属性
* 输入是一个只读信号
* `<name> = input<type>(<default>)` 定义输入
* `<name> = input.required(<default>)` 定义必要输入
* `input(<default>, {required: <func>})` 配置转换
* `input(<default>, {alias: <name>})` 定义别名


#### 示例
```ts
@Component({/*...*/})
export class CustomSlider {
  value = input(0);   //定义
}

<custom-slider [value]="50" />  //使用
```

### 输出事件
* `<name> = output<type>()`  定义事件
* `<name>.emit()`            发送事件
* `<anem>.emit(<val>)`       发送事件数据
    * `$event`  接收数据
* `output({alias: <name>})`  定义别名                


#### 示例
```ts
@Component({/*...*/})
export class ExpandablePanel {
  panelClosed = output<void>();  //定义
}

this.panelClosed.emit();  //发送事件
this.valueChanged.emit(7);  //发送事件数据

<expandable-panel (panelClosed)="savePanelState()" />  //监听
<custom-slider (valueChanged)="logValue($event)" />    //监听，接收数据
```
