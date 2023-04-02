

# QueryDSL

[TOC]


## 查询

### 简单
* `<xxx[]> = select(qm.xxx).from(qm).fetch()`  查询一个字段
* `<tuple[]> = select(qm.xxx, qm.yyy).from(qm).fetch()`  查询多个字段
* `selectFrom(qm).fetch()`           查询实体

* `selectDistinct(qm.xxx).from(qm).fetch()` 去重查询
* `selectFrom(qm).fetchFirst()`  获取首条结果
* `selectFrom(qm).fetchOne()`    查询唯一结果（有多条会出异常）


### 返回自定义对象
```kotlin
    select(
        qm.xxx, qm.yyy, qm.zzz
    ).from(qm)
    .fetch().stream()
    .map(tuple -> Custom(tuple.get(qm.xxx), tuple.get(qm.yyy), tuple.get(qm.zzz)))
    .collect(Collectors.toList())
```


## 删除

* `delete(qm).execute()`  删除表
* `delete(qm).where(xxx).execute()` 删除特定记录


##

* `update(qm).where(xxx).set(qm.xxx, <val>).execute()` 更新特定记录