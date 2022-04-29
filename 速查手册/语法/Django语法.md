

# Django语法

* 基于版本3.2

[TOC]

## 数据库

### 模型
* 一个模型对应一张表
* 一个模型是一个类，属性对应表的字段

#### 类型
* `IntegerField` 整型
* `CharField`   字符串
* `TextField`   大量文本
* `DateField`   日期，值为datetime.date实例
	* `auto_now`  每次保存时，自动设置为当前时间
	* `auto_now_add` 第一次创建对象时，自动设置为当前时间
* `BooleanField`  布尔，值为true/false
* `FloatField`  浮点数

* `ForeignKey(<table>)`      创建多对一关联
* `ManyToManyField(<table>)` 创建多对多关联
* `OneToOneField(<table>)`   创建一对一关联


#### 选项
* `db_column`  数据库列名
* `primary_key` 是否为主键
* `default`  默认值
* `unique`   值是否唯一
* `null`     是否将空值存储为NULL
* `db_index`  是否创建数据库索引
* `on_delete`  删除后操作
	* `CASCADE`     删除关联数据
	* `DO_NOTHING`  无操作
	* `PROTECT`     抛出异常

#### 元数据
* 使用内部`Meta`类赋予
* `abstract` 是否是抽象基类
* `db_table`  数据库表名
* `ordering`  默认排序
	* `fieldName` 按字段名升序排列
	* `-fieldName` 按字段名降序排列
 

#### 示例
```python
from django.db import models
class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)

	class Meta:
        ordering = ["first_name"]
```


### 操作
* `Entry(...).save()`  保存记录
* `<qset> = Entry.objects.all()` 获取所有记录
* `<qset> = Entry.objects.filter(...)`  获取满足过滤条件的记录
* `<qset> = Entry.objects.exclude(...)`  获取不满足过滤条件的记录
* `<qset> = Entry.objects.order_by(<field>)` 排序
* `<qset> = Entry.objects.distinct(<field>)` 去重
* `<qset> = Entry.objects.values(<field>)`   获取字典数据，而非实例

* `<int> = <qset>.count()`  获取匹配的记录数量
* `<bool> = <qset>.exists()` 检查匹配结果是否存在

* `<obj> = Entry.objects.get(...)`  获取满足条件的单个记录对象
* `<id> = Entry.objects.create(...)`  创建对象并保存
	* 等价于 `Entry(...).save(force_insert=True)`
* `objs = Entry.objects.bulk_create([...])` 批量创建
* `(<obj>, <bool>) = Entry.objects.get_or_create(..., default)` 查找特定对象，若无则创建
	* 等价于
		```python
		try :
			obj = Entry.objects.get(...)
		except Entry.DoesNotExist :
			obj = Entry(...)
			obj.save()
		```
* `<dict> = <qset>.aggregate(Avg(<field>))` 计算字段的平均值
	* 结果字段名 `<field>_avg`
* `<dict> = <qset>.aggregate(Max(<field>))` 计算字段的最大值
	* 结果字段名 `<field>_max`
* `<dict> = <qset>.aggregate(Min(<field>))` 计算字段的最小值
	* 结果字段名 `<field>_min`
* `<dict> = <qset>.aggregate(Count(<field>))` 计算字段的数量
* `<dict> = <qset>.aggregate(Count(<field>, distinct=True))` 计算字段非重复的数量
	* 结果字段名 `<field>_count`


#### 查找表达式
* 用于构建WHERE子句
* 格式：`<field>__<lookuptype>=<value>`

#### <lookuptype>
* `exact` 精确查找，默认类型
* `iexact`  不区分大小写
* `contains` 包含查找
* `icontains` 不区分大小写
* `startswith` 开头查找
* `endswith`  结尾查找
* `in`   在列表中查找
* `gt`   大于
* `gte`  大于等于
* `lt`   小于
* `lte`  小于等于
* `range`  范围查找
* `year`  精确匹配年份


#### 组合
* `Q(...) & Q(...)`  与条件
* `Q(...) | Q(...)`  或条件
* `~Q(...)`          取反条件


#### 示例
```python
Entry.objects.get(id__exact=None)
# ==》 SELECT ... WHERE id IS NULL;


Entry.objects.filter(pub_date__lte='2006-01-01')
# ==》SELECT * FROM blog_entry WHERE pub_date <= '2006-01-01';

Entry.objects.get(headline__contains='Lennon')
# ==》 SELECT ... WHERE headline LIKE '%Lennon%';

Entry.objects.filter(id__in=[1, 3, 4])
# ==》 SELECT ... WHERE id IN (1, 3, 4);

Entry.objects.filter(pub_date__range=(datetime.date(2005, 1, 1), datetime.date(2005, 3, 31)))
# ==》 SELECT ... WHERE pub_date BETWEEN '2005-01-01' and '2005-03-31';

Entry.objects.filter(pub_date__year=2005)
# ==》 SELECT ... WHERE pub_date BETWEEN '2005-01-01' AND '2005-12-31';

obj, created = Person.objects.get_or_create(
    first_name='John',
    last_name='Lennon',
    defaults={'birthday': date(1940, 10, 9)},
)

objs = Entry.objects.bulk_create([
    Entry(headline='This is a test'),
    Entry(headline='This is only a test'),
])


Book.objects.all().aggregate(Avg('price'))
# result : {'price__avg': 34.35}
```



## HTTP请求

### 配置路由
* `path(<str>, <module>.<function>)` 配置一个字符串路由给函数
* `path(<str>, include(<module>)`    配置一个字符串路由给模块
* `re_path(<regex>, <module>.<function>)` 配置一个正则表达式路由给函数
* `re_path(<regex>, include(<module>))` 配置一个正则表达式路由给模块

#### path路由
* `name`  匹配字符串，除了字符串`/`
* `<name>` 匹配并捕获字符串，name作为调用参数
* `<int:name>` 匹配并捕获正整数，name作为调用参数
* `<slug:name>`  匹配并捕获短标签，包含：ASCII字母、数字、连字符、下划线
* `<path:name>`  匹配并捕获完整路径，包含字符串`/`


#### 示例
```python
urlpatterns = [
    path('articles/2003/', views.special_case_2003),
    path('articles/<int:year>/', views.year_archive),
    path('articles/<int:year>/<int:month>/', views.month_archive),
	# /articles/2005/03/  ==> views.month_archive(request, year=2005, month=3)
    path('articles/<int:year>/<int:month>/<slug:slug>/', views.article_detail),
	# /articles/2003/03/building-a-django-site/  ==> views.article_detail(request, year=2003, month=3, slug="building-a-django-site")

	re_path(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),
    re_path(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/$', views.month_archive),
    re_path(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/(?P<slug>[\w-]+)/$', views.article_detail),
]
```


### 请求对象
* `<req>.method`  使用的HTTP方法，大写字母
* `<req>.scheme`  协议类型，http,https
* `<req>.path`    URL的完整路径
* `<req>.body`    POST的请求数据
* `<req>.headers`  请求头信息，一个字典
* `<req>.GET`      GET请求的参数，一个字典
* `<req>.POST`     POST请求的参数，一个字典
* `<req>.FILES`    所有上传的文件，一个字典
	* 键：`<input type="file" name="xxx">`
	* 值：`UploadedFile`对象


### 响应对象
* `HttpResponse` 生成响应
	* `content` 设置内容
	* `headers` 设置头信息
	* `charset` 设置字符编码
	* `status_code` 设置HTTP状态码
* `HttpResponseRedirect` 重定向响应
* `HttpResponseBadRequest` 400状态码响应
* `HttpResponseNotFound`  404状态码响应
* `JsonResponse`  JSON响应
* `FileResponse`  文件数据响应

#### 示例
```py
resp = HttpResponse("Here's the text of the Web page.")
resp = HttpResponse("Text only, please.", content_type="text/plain")
resp = HttpResponse(headers={'Age': 120})

resp = JsonResponse({'foo': 'bar'})
resp = JsonResponse([1, 2, 3], safe=False)

resp = FileResponse(open('myfile.png', 'rb'))
```



## 配置

### 操作
* `from django.conf import settings`  引入配置，一个对象

### 配置项
* `ALLOWED_HOSTS` 可以服务的主机、域名列表（安全配置）
* `INSTALLED_APPS` 所有被启用的应用程序
* `DATABASES`      数据库配置，必须设置一个`default`数据库
	* `ENGINE`     数据库后端
	* `HOST`       数据库主机
	* `PORT`       数据库端口号
	* `NAME`       数据库名字
* `CACHES`         缓存配置，必须设置一个`default`缓存
	* `BACKEND`    缓存后端




#### 示例
```py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'mydatabase',
        'USER': 'mydatabaseuser',
        'PASSWORD': 'mypassword',
        'HOST': '127.0.0.1',
        'PORT': '5432',
    }
}
```
