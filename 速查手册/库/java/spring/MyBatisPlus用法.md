


# MyBatisPlus用法


## 接口

### IService
* `boolean save(<entity>)`  插入记录
* `boolean saveBatch(<entitys>, [<size>])` 批量插入,设置批次大小
* `boolean saveOrUpdate(<entity>)` 插入或更新记录
* `boolean saveOrUpdateBatch(<entitys>, [<size>])` 批量插入或更新,设置批次大小

* `boolean removeById(<id>)` 根据ID删除
* `boolean removeByIds(<ids>)` 根据多个ID批量删除
* `boolean remove(<wrapper>)` 根据查询语句删除
* `boolean removeByMap(<columnMap>)` 根据列映射删除

* `boolean update(<wrapper>)` 根据语句更新
* `boolean updateById(<entity>)` 根据ID更新对象
* `boolean updateBatchById(<entitys>, [<size>])` 根据ID批量更新

* `<obj> getById(<id>)`  根据ID获取对象
* `<obj> getOne(<wrapper>, <throwEx>)` 根据查询语句获取对象
    * `<throwEx>`: 多个对象是否抛出异常

* `<objs> = list()` 查询所有记录
* `<objs> = list(<wrapper>)` 根据查询语句查询
* `<objs> = listByIds(<ids>)` 根据多个ID查询

* `<page> = page(<pageParam>)` 分页查询
* `<page> = page(<pageParam>, <wrapper>)` 根据查询语句，分页查询

* `<size> = count()`  获取总记录数
* `<size> = count(<wrapper>)` 根据查询语句获取记录数
  

#### 示例
```java
QueryWrapper<User> queryWrapper = new QueryWrapper<>();
queryWrapper.eq("name", "John Doe");
userService.remove(queryWrapper);


UpdateWrapper<User> updateWrapper = new UpdateWrapper<>();
updateWrapper.eq("name", "John Doe").set("email", "john.doe@newdomain.com");
userService.update(updateWrapper);

QueryWrapper<User> queryWrapper = new QueryWrapper<>();
queryWrapper.eq("name", "John Doe");
User user = userService.getOne(queryWrapper);


QueryWrapper<User> queryWrapper = new QueryWrapper<>();
queryWrapper.gt("age", 25);
List<User> users = userService.list(queryWrapper);

Collection<User> users = userService.listByIds(Arrays.asList(1, 2, 3));

IPage<User> page = new Page<>(1, 10); //第一页，每页10条记录
IPage<User> userPage = userService.page(page);
List<User> userList = userPage.getRecords();
long total = userPage.getTotal();

IPage<User> page = new Page<>(1, 10);
QueryWrapper<User> queryWrapper = new QueryWrapper<>();
queryWrapper.gt("age", 25);
IPage<User> userPage = userService.page(page, queryWrapper);  
List<User> userList = userPage.getRecords();
long total = userPage.getTotal();

QueryWrapper<User> queryWrapper = new QueryWrapper<>();
queryWrapper.gt("age", 25);
int totalUsers = userService.count(queryWrapper);
```


### Mapper
* `<int> = insert(<entity>)`  插入记录
    * 返回值：插入影响的行数，1表示插入成功

* `<int> = deleteById(<id>)`  根据ID删除记录
    * 返回值：插入影响的行数，1表示删除成功
* `<int> = deleteBatchIds(<ids>)` 
* `<int> = delete(<wrapper>)` 根据查询语句删除记录

* `<int> = updateById(<entity>)` 根据ID更新记录
* `<int> = update(<entity>, <wrapper>)` 根据查询语句，更新记录

* `<obj> = selectById(<id>)` 根据ID查询记录
* `<obj> = selectOne(<wrapper>)` 根据语句查询一条记录
* `<objs> = selectBatchIds(<ids>)` 根据ID批量查询记录
* `<objs> = selectList(<wrapper>)` 根据语句查询多条记录
* `<page> = selectPage(<pageParam>, <wrapper>)` 根据语法查询分页记录

* `<size> = selectCount(<wrapper>)` 根据语句获取记录数


#### 示例
```java
QueryWrapper<User> queryWrapper = new QueryWrapper<>();
queryWrapper.gt("age", 25);
int rows = userMapper.delete(queryWrapper);

int rows = userMapper.deleteBatchIds(Arrays.asList(1, 2, 3));

UpdateWrapper<User> updateWrapper = new UpdateWrapper<>();
updateWrapper.gt("age", 25);
User updateUser = new User();
updateUser.setEmail("new.email@example.com");
int rows = userMapper.update(updateUser, updateWrapper);

User updateUser = new User();
updateUser.setId(1);
updateUser.setEmail("new.email@example.com");
int rows = userMapper.updateById(updateUser);


QueryWrapper<User> queryWrapper = new QueryWrapper<>();
queryWrapper.gt("age", 25);
User user = userMapper.selectOne(queryWrapper);


IPage<User> page = new Page<>(1, 10);
QueryWrapper<User> queryWrapper = new QueryWrapper<>();
queryWrapper.gt("age", 25);
IPage<User> userPage = userMapper.selectPage(page, queryWrapper);
List<User> userList = userPage.getRecords();
long total = userPage.getTotal();
```


## 条件构造

### 类
* `<QueryWrapper> = Wrappers.query()` 构造查询条件
* `<UpdateWrapper> = Wrappers.update()` 构造更新条件
* `<LambdaQueryWrapper> = Wrappers.lambdaQuery()` 构造lambda的查询条件
* `<LambdaUpdateWrapper> = Wrappers.lambdaUpdate()` 构造lambda的更新条件
* 方法会带一个`<bool>`参数，决定该条件是否加入到sql中


### 条件
* `allEq(<map>)`  多字段相等
* `allEq(<cond>, <map>)` 带条件的多字段相等
* `allEq(<filter>, <map>)` 带字段过滤的多字段相等

* `eq(<col>, <val>)` 单字段相等
* `eq(<cond>, <col>, <val>)` 带条件的单字段相等

* `ne(<col>, <val>)` 单字段不相等
* `ne(<cond>, <col>, <val>)` 带条件的单字段不相等

* `lt(<col>, <val>)` 单字段小于
* `lt(<cond>, <col>, <val>)` 带条件的单字段小于

* `between(<col>, <val1>, <val2>)`
* `between(<cond>, <col>, <val1>, <val2>)`

* `notBetween(<col>, <val1>, <val2>)`
* `notBetween(<cond>, <col>, <val1>, <val2>)`

* `like(<col>, <val>)`
* `likeLeft(<col>, <val>)`
* `likeRight(<col>, <val>)`

* `isNull(<col>)`  设置IS NULL条件

* `in(<col>, <vals>)`
* `notIn(<col>, <vals>)`


* `and(<param>)`  添加与嵌套条件
* `and(<cond>, <param>)` 添加与嵌套条件

* `or()`       后续条件改为或
* `or(<cond>)` 后续条件改为或

* `or(<param>)`  添加或嵌套条件
* `or(<cond>, <param>)` 添加或嵌套条件


* `select(<field>, ...)` 设置查询字段


#### 示例
```java
queryWrapper.allEq(Map.of("id", 1, "name", "老王", "age", null));
queryWrapper.allEq((field, value) -> field.contains("a"),
                   Map.of("id", 1, "name", "老王", "age", null));

queryWrapper.eq("name", "老王");
lambdaQueryWrapper.eq(User::getName, "老王");


lambdaQueryWrapper.ne(User::getName, "老王");

queryWrapper.between("age", 18, 30);
lambdaQueryWrapper.between(User::getAge, 18, 30);
//SELECT * FROM user WHERE age BETWEEN 18 AND 30

queryWrapper.notBetween("age", 18, 30);
lambdaQueryWrapper.notBetween(User::getAge, 18, 30);
//SELECT * FROM user WHERE age NOT BETWEEN 18 AND 30


queryWrapper.like("name", "王");
//SELECT * FROM user WHERE name LIKE '%王%'

queryWrapper.likeLeft("name", "王");
//SELECT * FROM user WHERE name LIKE '%王'

queryWrapper.isNull("name");
//SELECT * FROM user WHERE name IS NULL


queryWrapper.in("age", Arrays.asList(1, 2, 3));
//SELECT * FROM user WHERE age IN (1, 2, 3)

queryWrapper.notIn("age", Arrays.asList(1, 2, 3));
//SELECT * FROM user WHERE age NOT IN (1, 2, 3)


queryWrapper.and(i -> i.and(j -> j.eq("name", "李白").eq("status", "alive"))
            .and(j -> j.eq("name", "杜甫").eq("status", "alive")));
//SELECT * FROM user WHERE ((name = '李白' AND status = 'alive') 
//                           AND (name = '杜甫' AND status = 'alive'))


queryWrapper.eq("id", 1).or().eq("name", "老王");
//SELECT * FROM user WHERE id = 1 OR name = '老王'


queryWrapper.or(i -> i.and(j -> j.eq("name", "李白").eq("status", "alive"))
            .or(j -> j.eq("name", "杜甫").eq("status", "alive")));
//SELECT * FROM user WHERE (name = '李白' AND status = 'alive')
//                          OR (name = '杜甫' AND status = 'alive')


queryWrapper.select("id", "name", "age");
//SELECT id, name, age FROM user

```