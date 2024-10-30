


# crane4j用法


[TOC]



## 填充对象


### 定义填充操作
使用标注：`@Assemble`

#### 参数
* `container` 需要使用的数据源容器
* `handlerType` 装配处理器
* `props, prop` 属性映射
* `propertyMappingStrategy` 属性映射策略


#### 属性映射

##### 属性到属性
`将源对象上的属性映射到目标对象的属性上`
* `props = @Mapping(src = "a", ref = "b")` 
* `prop = "a:b"` 

##### 对象到属性
`将整个源对象映射到目标对象的属性上`
* `props = @Mapping(ref = "b")`
* `prop = ":b"`


#### 属性映射策略
* `OverwriteMappingStrategy` 覆写
    * 无论`src`的属性值是否为空，都覆写`ref`的属性
* `OverwriteNotNullMappingStrategy` 非空时覆写（默认策略）
    * 仅当`src`的属性值不为空时，才覆写`ref`的属性
* `ReferenceMappingStrategy` 空值引用
    * 仅当`ref`的属性值为空时，才使用`src`的属性值进行覆写

    

#### 装配类型
* `OneToOneAssembleOperationHandler` 一对一
    * 一个属性值对应一个数据源对象
* `OneToManyAssembleOperationHandler` 一对多
    * 一个属性值对应多个数据源对象
* `ManyToManyAssembleOperationHandler` 多对多
    * 多个属性值对应多个数据源对象


##### 图解

一对多
![](http://picbed.cc12703.com/20240905232110.png)

多对多
![](http://picbed.cc12703.com/20240905233136.png)


##### 示例
```java

//一对一
public class Foo {
    @Assemble(container = "foo", handlerType = OneToOneAssembleOperationHandler.class)
    private String name;
    private String alias;
}


//一对多，customer容器会返回按组id分组的客户集合
public class CustomerVO {
    @Assemble(
        container = "customer",
        handlerType = OneToManyAssembleOperationHandler.class,
        props = @Mapping(src = "name", ref = "customerNames")
    )
    private Integer groupID;
    private List<String> customerNames;
}

//多对多
public class StudentVO {
    @Assemble(
        container = "teacher", 
        handlerType = ManyToManyAssembleOperationHandler,
        props = @Mapping(src = "name", ref = "teacherNames")
    )
    private String teacherIds; // 默认支持 "1, 2, 3" 格式
    private List<String> teacherNames; // 填充的格式默认为 src1, src2, src3
}

//多对多
public class StudentVO {
    @Assemble(
        container = "teacher", 
        props = @Mapping(ref = "teachers"),
        handlerType = ManyToManyAssembleOperationHandler
    )
    private String teacherIds; // 默认支持 "1, 2, 3" 格式
    private List<Teacher> teachers;
}

```


### 条件填充

* `@ConditionOnExpression(value=xxx)` 根据表达式的值判断是否要填充
    * spring中表达式语法为`SpEL`
* `@ConditionOnPropertyNotEmpty` 当属性不为空时才填充


### 回调函数

* `OperationAwareBean` 定义了装配前后的回调函数
    * `beforeAssembleOperation` 填充前调用
    * `afterOperationsCompletion` 填充后调用


#### 示例
```java
@Data
private static class Bean implements OperationAwareBean {

    @Assemble(container = "user", props = @Mapping("name"))
    private Integer id;
    private String name;
    @Assemble(container = "entry", props = @Mapping("value"))
    private Integer key;
    private String value;
    @Disassemble(type = NestedBean.class)
    private NestedBean nestedBean;
    private String remark;

    @Override
    public boolean supportOperation(String key) {
        // 若 nestedBean 为空，则不进行针对 nestedBean 属性的递归填充
        return !Objects.equals("nestedBean", key) 
            || Objects.nonNull(this.nestedBean);
    }

    @Override
    public void beforeAssembleOperation() {
        // 在填充开始前，若 id 为空，则为其设置一个默认值
        if (Objects.isNull(this.id)) {
            this.id = 0; 
        }
    }

    @Override
    public void afterOperationsCompletion() {
        // 在完成填充后，对属性值做一些处理
        this.name = "尊敬的：" + this.name;
    }
}

```



## 定义数据源

### 方法容器
使用标注：`@ContainerMethod()`

#### 参数
* `namespace` 定义数据源名字
* `type`  结果分组类型
* `resultKey` 用于分组的键
* `resultType` 返回值类型
* `filterNullKey` 是否过滤`null`值


#### 分组类型
* `ONE_TO_ONE` 按键一对一分组，结果`Map<key, value>`
* `ONE_TO_MANY` 按键一对多分组，结果`Map<key, List<value>>`
* `NO_MAPPING` 不进行分组，返回值就是`Map`
* `ORDER_OF_KEYS` 将键值与结果按顺序合并，结果`Map<key, value>`





##### 示例
```java
// 查询用户，并按用户 ID 一对一分组
@ContainerMethod(
    namespace = "userName", type = MappingType.ONE_TO_ONE,
    resultType = User.class, resultKey = "userId"
)
public List<User> listUserByIds(List<Integer> ids); 

// 查询用户，并按用户的所属部门 ID 一对多分组
@ContainerMethod(
    namespace = "userName", type = MappingType.ONE_TO_MANY,
    resultType = User.class, resultKey = "deptId"
)
public List<User> listUserByDeptId(List<Integer> deptIds);
```