
# Android-Room语法


[TOC]


## 定义实体

### 标注
* `@Entity`      定义实体类
    * `tableName`  表名
    * `primaryKeys` 定义复合主键
* `@PrimaryKey`  定义主键字段
    * `autoGenerate` 自动生成主键
* `@ColumnInfo`   定义数据字段
    * `name`      字段名
    * `defaultValue` 默认值
* `@Ignore`       忽略该字段
* `@Embedded`     定义内嵌对象 
    * `prefix`    前缀名


### 示例
```java

```


## Dao访问数据