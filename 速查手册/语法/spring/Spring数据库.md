


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
