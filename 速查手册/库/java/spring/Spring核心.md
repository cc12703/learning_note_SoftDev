

# Spring核心

[TOC]



## IoC (Inversion of Control)

### 标注

* `@Component` 	定义通用组件，用于类
* `@Controller` 定义controller类，用于类
* `@Service` 	定义service类，用于类
* `@Repository` 定义Dao类，用于类
* `@Bean`       生成Bean对象，用于方法

* `@PostConstruct` 	标注初始化方法
* `@PreDestroy`  	标注销毁方法

* `@Value` 		注入普通值
* `@Resource` 	根据名字注入对象
* `@Autowired`  根据类型注入对象，可用于构造器、字段、方法
	* `@Primary`    优先注入
	* `@Qualifier`  根据名字来注入
* `@Scope`  	设置作用范围
	* `singleton` 单例
	* `prototype` 多例

* `@ComponentScan` 标明采样什么策略去扫描装配Bean


#### 示例
```kotlin

//字段注入
@Autowired
private User user;

//方法注入
@Autowired
public void initCity(City city) {
	this.city = city;
}

//构造函数注入
class UserHolder {

	@Autowired
	public UserHolder(User user) {
		this.user = user;
	}
}


//生成Bean对象
@Bean
public AccountDao accountDao(){
	return new AccountDao();
}
```



## Web

### 标注
* `@RequestMapping` 关联请求URL
	* 标注在类上，为第一级访问目录
	* 标注在方法上，为第二级访问目录
	* `path` 请求路径
	* `method`  请求方式
	* `params`  限制条件，请求参数
	* `headers` 限制条件，请求头
* `@ResponseBody` 标注方法返回对象

* `@GetMapping` 关联GET请求
* `@PostMapping` 关联POST请求
* `@PutMapping` 关联PUT请求
* `@DeleteMapping` 关联DELETE请求

* `@RequestBody`  标注参数，提取请求体内容
* `@PathVariable` 标注参数，提取URL中的变量参数
* `@RequestParam` 标注参数，提取HTTP的请求参数
	* `value` 请求参数名字
	* `required` 是否必须
	* `defaultValue` 默认值

* `@RestController` 相当于 `@Controller`和`@ResponseBody`


#### 示例
```kotlin
@RestController
@RequestMapping("test")
class XXXController {

	@GetMapping("get/{id}")
	fun getVehicle(@PathVariable("id") id: Long, 
					@RequestParam("name") name: String): Vehicle {
		// ...
	}
}
//url: xxx/test/get/1?name=xxx
```



