


# Spring数据库


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
	* 方法：使用外键来建立关系，由定义方来维护关系
	* `fetch` 抓取策略
		* `EAGER`  立即加载
		* `LAZY`   懒加载
	* `cascade` 级联操作策略（给当前实体操作另一个实体的权限）
		* `PERSIST` 保存当前实体时，与其有映射关系的实体也会被保存
		* `REMOVE` 删除当前实体时，与其有映射关系的实体也会被删除
		* `MERGE`  更新当前实体数据时，与其有映射关系的实体也会被更新
		* `DETACH` 删除当前实体时，与其相关的外键都会被撤销
		* `REFRESH` 刷新当前实体时，也刷新有映射关系的实体
	* `mappedBy` 引用关系维护字段


* `@ManyToOne` 描述多对一映射
	* `fetch` 抓取策略
	* `cascade` 级联操作策略

* `@JoinColumn`  定义关系维护字段

* `@OneToMany` 描述一对多映射
	* `fetch` 抓取策略
	* `cascade` 级联操作策略
	* `mappedBy` 引用关系维护字段


* `@ManyToMany` 描述多对多映射
	* 方法：使用中间表建立关系，两张表和中间表都是一对多的关系
	* `fetch` 抓取策略
	* `cascade` 级联操作策略
	* `mappedBy`  引用关系拥有放的字段
* `JoinTable`  配置中间表
	* `name`  表名
	* `joinColumns` 中间表的外键字段关联当前类的主键字段
	* `inverseJoinColumn` 中间表的外键字段关联对方表的主键字段



### 示例
一对一映射
```kotlin
class Book {

	@OneToOne()
	@JoinColumn(name = "book_detail_id")
	detail: BookDetail
}


class BookDetail {

	@OneToOne(mappedBy="detail")
	book: Book
}
```


一对多映射
```kotlin
class Teacher {

	@OneToMany(mappedBy="techder")
	students: List<Student>
}


//多方
class Student {

	@ManyToOne()
	@JoinColumn(name = "teacher_id")
	teacher: Teacher
}
```

多对多映射
```kotlin
class User {

	@ManyToMany()
	@JoinTable(name = "user_role")
	roles: List<Role>
}

class Role {

	@ManyToMany(mappedBy="roles")
	users: List<User>
}
```