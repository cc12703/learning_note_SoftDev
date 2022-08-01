

# Spring语法

[TOC]



## IoC (Inversion of Control)

### 注解

#### 定义Bean类
* `@Component` 标注通用组件
* `@Controller` 标注controller实现类
* `@Service` 标注service实现类
* `@Repository` 标注Dao实现类


#### Bean生命周期
* `@PostConstruct` 标注初始化方法
* `@PreDestroy`  标注销毁方法


#### 注入Bean
* `@Value` 注入普通值
* `@Resource` 根据名字注入对象
* `@Autowired`  根据类型注入对象
	* `@Primary`    优先注入
	* `@Qualifier`  根据名字来注入
* `@Scope`  设置作用范围
	* `singleton` 单例
	* `prototype` 多例

#### 其他
* `@ComponentScan` 标明采样什么策略去扫描装配Bean



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
	* `cascade` 级联操作策略
* `@ManyToOne` 描述多对一映射
	* `fetch` 抓取策略
	* `cascade` 级联操作策略
* `@OneToMany` 描述一对多映射
	* `fetch` 抓取策略
	* `cascade` 级联操作策略
* `@ManyToMany` 描述多对多映射