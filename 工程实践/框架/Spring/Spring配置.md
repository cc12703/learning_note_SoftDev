

# Spring配置

[TOC]


## 概述
* 配置属性是Spring应用上下文中bean的属性
* 可以通过多个源进行设置

#### 环境抽象
* Spring本身抽取了原始的属性，需要属性的bean可以从Spring中获取
* 属性源
    * JVM系统属性
    * 操作系统的环境变量
    * 命令行参数
    * 应用属性配置文件
        * application.properties
        * application.yml

##### 图示
![](http://picbed.cc12703.com/20210223182548.png)



## 系统配置

### 文件
* `application.properties`
* `application.yml`

#### 要点
* properties默认使用ISO8859-1编码
	* 使用中文时，需要加入`spring.http.encoding.charset=UTF-8`
* properties优先级高于yml
* properties支持`@PropertySource`


### 配置示例

#### 数据源
```yml
spring:
    datasource:
        url: jdbc:mysql://localhost/tacocloud 
        username: tacodb
        password: tacopass 
        driver-class-name: com.mysql.jdbc.Driver 

        schema:  #创建表结构
        - order-schema.sql
        - user-schema.sql

        data:   #填充数据
        - ingredient.sql
```

#### 服务器
```yml
server:
    port: 8443
    ssl: 
        key-store: file://path/mykeys.jks
        key-store-password: letmein
        key-password: letmein
```

#### 日志
```yml
loggin:
    path: /var/logs/
    file: TacoCloud.log
    level:  # 设置日志级别
        root: WARN
        org.springframework.security: DEBUG
```


## 自定义配置


### @Value

#### 要点
* 用于设置简单的配置项
* 被标注的类必须被Spring容器管理
* 需要传入完整的配置项键
* 默认读取properties文件


#### 示例
```kotlin
@Value("${com.weiz.costum.firstname}")
com.weiz.resource.name=weiz
private val firstName: String
```

### Environment

#### 要点
* 该类会自动获取系统加载的所有配置项

#### 示例
```kotlin
@Autowired
private val env: Enviromnet


fun test() {
	val cfgVal = env.getProperty("com.weiz.costum.firstname")
}
```


### @ConfigurationProperties


#### 要点
* 将配置项和实体Bean关联起来
* 用于创建非常多的配置项

#### 示例
```kotlin
@Configuration
@ConfigurationProperties(prefix="com.weiz.resource")
@PropertySource(value = "classpath:website.properties")
public class OrderProps {

    private val name: String
	private val website: String

}
```

website.properties
```properties
com.weiz.resource.name=weiz
com.weiz.resource.website=www.weiz.com
```


## 多环境

### 环境
* 开发环境：`dev`
* 测试环境：`test`
* 生产环境：`prod`


### 命名
* `application-{profile}.properties` 环境配置文件
* `application.properties`           主配置文件（公共配置项）


### 激活
* `spring.profiles.active` 配置项（不推荐）
* `SPRING_PROFILES_ACTIVE` 环境变量（推荐）


## 其他

### 随机数
* 加载配置文件时，可以自动生成随机数
* 是用格式：`${random}`


#### 示例
```java
${random.value}  //随机字符串
${random.uuid}   //UUID
${random.int}    //随机整数
${random.int[10,20]}  //10-20的随机数
```

### 配置引用
* 通过占位符引用定义过的配置项
* 格式：`${name:default}`


#### 示例
```properties
my.name=zzzz
my.sex=1
my.des=My name is ${my.name:weiz}
```