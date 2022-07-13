

# SpringWeb


[TOC]


## 注解

### @Controller
* 标注类为控制器，可以返回数据或页面
* 要返回数据，需要使用`@ResponseBody`


### @RestController
* 标注类为控制器，只能返回数据
* 相当于`@Controller`和`@ResponseBody`的结合


### @RequestMapping
* 负责URL的路由映射
* 若在控制类上，则类中的所有方法都会加上该映射
* 若在方法上，则只有该方法才会加上该映射

#### 参数
* `value` 请求的路径，支持模板和正则表达式
* `method` 请求方法
* `params` 请求参数
* `headers` 请求头的值


### @ResponseBody
* 定义数据的返回格式
* 默认使用Jackson序列化成json字符串


### @PathVariable
* 用于提取URL中的变量参数
* 将参数绑定到URL模板变量

#### 示例
```java
@RequestMapping("/{id}")
Vehicle getVehicle(@PathVariable("id") long id) {
    // ...
}
```

### @RequestParam
* 获取HTTP的请求参数

#### 示例
```java
@RequestMapping
Vehicle getVehicleByParam(@RequestParam("id") long id) {
    // ...
}
```


### @RequestBody
* 将传入的json数据映射为实体对象


### RESTful类型
* `@GetMapping` 处理GET请求
* `@PostMapping` 处理POST请求
* `@PutMapping` 处理PUT请求
* `@DeleteMapping` 处理DELETE请求


## URL映射

### 路径精确匹配
* 格式：`{name}`
* 使用`@PathVariable`提取参数


#### 示例
```kotlin
@RequestMapping("/getDataByID/{id}")
fun getDataById(@PathVariable("id") id: String) {
	//...
}
```


### 路径通配符匹配
* `*`  匹配任意字符
* `**` 匹配任意路径
* `?`  匹配单个字符

#### 优先级
* 有通配符的低于没有通配符的
* `**`低于`*`


#### 示例
```kotlin
@RequestMapping("/getJson/*.json")
fun getJson() {
	//...
}
```



## 拦截器

### 用途
* 权限检查：登录检测
* 性能监控
* 通用行为：读取用户信息


### HandlerInterceptor
* `preHandle`: 预处理方法
	* 返回true表示继续流程
	* 返回false表示中断流程，通过response产生响应
* `postHandle`：后处理方法
	* 仍然在渲染视图之前被调用
* `afterCompletion`：完成处理方法
	* 在视图渲染完成后被调用



## 异常处理

### @ExceptionHandler
* 在控制器类的中使用，处理局部异常
* 标注在需要处理异常的方法上


### SimpleMappingExceptionResolver
* 用于处理全局异常
* 所有异常都按统一方式处理



### @ControllerAdvice
* 用于处理全局异常
* 需要`@ExceptionHandler`配合


#### 示例
```kotlin
@ControllerAdvice
class GlobalExceptionHandler {

	@ExceptionHandler(value=[Exception.class])	
	fun errorHandler(req: Request, resp: Response, e: Exception) {
		//...
	}
}
```