

# TypeORM用法


## 功能
* 一个数据库的ORM框架



## 启动

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


## 实体

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


## 实体关系

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





## 查询


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