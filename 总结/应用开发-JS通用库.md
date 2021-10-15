

# 应用开发-JS通用库


[TOC]


## Axios
* [参考手册](https://axios-http.com/docs/intro)


### 功能
* 支持Promise的网络请求库
* 支持浏览器和node.js平台


### 创建实例
```ts
const instance = axios.create({
	//基础链接地址
	baseURL: 'https://some-domain.com/api/',  
	//连接超时时间
	timeout: 1000,
	//自定义请求头
	headers: {'X-Custom-Header': 'foobar'}
	})
```

### 发送GET请求
```ts
axios.get('/user?ID=12345')
  .then((resp) => { 
	  //处理成功情况 
	  resp.data  //返回的响应数据
	  resp.status  //返回的HTTP状态码
	  resp.statusText //返回的HTTP状态信息
	  resp.headers   //返回的响应头
  })
  .catch((error) => { //处理错误情况 })
  .then(() => { //总是会执行 })
```

### 发送POST请求
```ts
axios.post('/user', { //body数据
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then((resp) => { //处理成功情况 })
  .catch((error) => { //处理错误情况 })
```

### 拦截器
```ts
// 请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前处理
    return config
  }, function (error) {
    // 处理请求错误
    return Promise.reject(error)
  })

// 响应拦截器
axios.interceptors.response.use(function (resp) {
    // 2xx内的状态码进入该函数
    // 处理响应数据
    return resp
  }, function (error) {
    // 2xx外的状态码进入该函数
    // 处理响应错误
    return Promise.reject(error)
  })
```

### 取消请求
```ts
const source = axios.CancelToken.source()

axios.get('/user/12345', {
  cancelToken: source.token
}).catch((error) => {
  if (axios.isCancel(error)) {
    console.log('Request canceled', error.message)
  } else {
    // 处理错误
  }
})

// 取消请求（message 参数是可选的）
source.cancel('Operation canceled by the user.')
```


## TypeORM

* [文档](https://www.bookstack.cn/read/TypeORM-0.2.20-zh/README.md)
* [手册](https://typeorm.io/)

### 功能
* 一个数据库的ORM框架


### 创建实体
```ts
@Entity()
export class Photo {
  @PrimaryColumn() //标记主列
  id: number

  @Column() //标记普通列
  name: string

  @Column()
  views: number

  @Column()
  isPublished: boolean
}
```

### 创建嵌入实体
```ts
export class Name {
    @Column()
    first: string

    @Column()
    last: string
}

@Entity()
export class User {
    @PrimaryGeneratedColumn()
    id: string

    @Column(type => Name)
    name: Name

    @Column()
    isActive: boolean
}
```

### 创建实体继承
```ts
export abstract class Content {
    @PrimaryGeneratedColumn()
    id: number

    @Column()
    title: string

    @Column()
    description: string
}

@Entity()
export class Photo extends Content {
    @Column()
    size: string
}
```

### 创建一对一关系
```ts
// 一个User实体对应一个Profile实体
@Entity()
export class Profile {
  @Column()
  gender: string

  @Column()
  photo: string
}

@Entity()
export class User {
  @Column()
  name: string

  @OneToOne(type => Profile)
  @JoinColumn()  //在表中插入外键
  profile: Profile
}
```

### 创建一对多关系
```ts
//一个User实体对应多个Photo实体
@Entity()
export class Photo {
  @Column()
  url: string

  @ManyToOne(type => User, user => user.photos)
  user: User
}

@Entity()
export class User {
  @Column()
  name: string

  @OneToMany(type => Photo, photo => photo.user)
  photos: Photo[]
}
```

### 创建多对多关系
```ts
@Entity()
export class Category {
  @PrimaryGeneratedColumn()
  id: number

  @Column()
  name: string
}

@Entity()
export class Question {
  @PrimaryGeneratedColumn()
  id: number

  @Column()
  title: string

  @Column()
  text: string

  @ManyToMany(type => Category)
  @JoinTable()
  categories: Category[]
}
```



### 连接数据库
```ts
createConnection({
  type: "mysql",    //数据库类型
  	host: "localhost",  //连接地址
  	port: 3306,         //连接端口号
  	username: "root",   //数据库用户名
  	password: "admin",  //数据库密码
  database: "test",   //数据库名
  entities: ["entity/*.js"],  //要加载的实体
  migrations: ["migration/*.js"], //要加载的迁移
  synchronize: true,  //启动时是否重建数据库架构
  logging: false    //是否启用日志记录
})
.then(connection => {
	// 连接成功
})
.catch(error => {
	// 连接失败
})
```


### QueryBuilder

#### 查询 -- 获取单个值
```ts
await getRepository(User)
  .createQueryBuilder("user")  //user 为别名
  .where("user.id = :id OR user.name = :name", { id: 1, name: "Timber" })
  .getOne()
```

#### 查询 -- 获取多个值
```ts
await getRepository(User)
  .createQueryBuilder("user")
  .getMany()
```

#### 查询 -- where条件
```ts
createQueryBuilder("user")
  .where("user.firstName = :firstName", { firstName: "Timber" })
  .andWhere("user.lastName = :lastName", { lastName: "Saw" })
//SELECT ... FROM users user WHERE user.firstName = 'Timber' AND user.lastName = 'Saw'


createQueryBuilder("user")
  .where("user.firstName = :firstName", { firstName: "Timber" })
  .orWhere("user.lastName = :lastName", { lastName: "Saw" })
//SELECT ... FROM users user WHERE user.firstName = 'Timber' OR user.lastName = 'Saw'


createQueryBuilder("user")
    .where("user.registered = :registered", { registered: true })
    .andWhere(new Brackets(qb => {
        qb.where("user.firstName = :firstName", { firstName: "Timber" })
          .orWhere("user.lastName = :lastName", { lastName: "Saw" })
//SELECT ... FROM users user WHERE user.registered = true 
// 				AND (user.firstName = 'Timber' OR user.lastName = 'Saw')
```

#### 查询 -- 排序
```ts
createQueryBuilder("user").orderBy("user.id")

//降序排序
createQueryBuilder("user").orderBy("user.id", "DESC")

//多个字段进行排序
createQueryBuilder("user")
  .orderBy("user.name")
  .addOrderBy("user.id")
```

#### 查询 -- 分页
```ts
await getRepository(User)
  .createQueryBuilder("user")
  .leftJoinAndSelect("user.photos", "photo")
  .skip(5)  //跳过前5个
  .take(10) //获取之后的10个
  .getMany()
```

#### 插入记录
```ts
getConnection()
  .createQueryBuilder()
  .insert()
  .into(User)   //插入数据的表
  .values([{ firstName: "Timber", lastName: "Saw" }, 
  			{ firstName: "Phantom", lastName: "Lancer" }])  //要插入的记录
  .execute()
```

#### 更新记录
```ts
getConnection()
  .createQueryBuilder()
  .update(User)   //更新数据的表
  .set({ firstName: "Timber", lastName: "Saw" })  //要更新的记录
  .where("id = :id", { id: 1 })
  .execute();]
```

#### 删除记录
```ts
getConnection()
  .createQueryBuilder()
  .delete()
  .from(User)
  .where("id = :id", { id: 1 })
  .execute()
```


## moment
* 功能： 日期处理
* [文档](http://momentjs.cn/)

### 功能
* 支持浏览器环境和Node.js环境




## uri.js


## cheerio
* 解析html文件
* jquery核心功能的简化实现
* https://www.jianshu.com/p/629a81b4e013
* https://cheerio.js.org/



## qs

* 处理url参数
* [github](https://github.com/ljharb/qs)

## xlsx

* 读写xlsx文件
* [文档](https://www.jianshu.com/p/31534691ed53)
* [github](https://github.com/SheetJS/sheetjs)

## xlsx-populate

* 读写xlsx文件
* [文档](https://github.com/dresende/xlsx-populate)