

# Spring语法

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


## JPA

### 注解
* `@Entity` 标明该类是一个实体类
	* 表名和类名相同
* `@Table`  标明表名
	* `name` 表名
* `@Column` 标明字段名
	* `name` 列名
	* `unique`  是否唯一
	* `nullable` 是否允许为null
	* `insertable` 是否允许插入
	* `updatetable` 是否允许更新
* `@Id`     标明为主键
* `GeneratedValue`  标明主键生成规则
	* `AUTO` 由程序自动生成，默认值
	* `IDENTITY` 由数据库自动生成
* `@Transient` 标明为忽略字段

* `@OneToOne` 描述一对一映射
	* `fetch` 抓取策略
	* `cascade` 级联操作策略（给当前实体操作另一个实体的权限）
		* `PERSIST` 保存当前实体时，与其有映射关系的实体也会被保存
		* `REMOVE` 删除当前实体时，与其有映射关系的实体也会被删除
		* `MERGE`  更新当前实体数据时，与其有映射关系的实体也会被更新
		* `DETACH` 删除当前实体时，与其相关的外键都会被撤销
		* `REFRESH` 刷新当前实体时，也刷新有映射关系的实体
* `@ManyToOne` 描述多对一映射
	* `fetch` 抓取策略
	* `cascade` 级联操作策略

* `@OneToMany` 描述一对多映射
	* `fetch` 抓取策略
	* `cascade` 级联操作策略

* `@ManyToMany` 描述多对多映射

