
# android应用-库

[TOC]

## Hilt

* [参考资料](https://developer.android.google.cn/training/dependency-injection?hl=zh-cn)
* [Hilt手册](https://dagger.dev/hilt/)

### 功能
* 构建在Dagger库之上的依赖注入库
* 提供了一种标准方式来使用依赖注入
* 根据对应android类的生命周期，自动创建和销毁组件实例
* 默认情况下组件没有作用域，每次需要绑定时都会创建一个实例


### 添加依赖项
* 一个gradle插件：hilt-android-gradle-plugin
* 一个运行库：hilt-android
* 一个编译库：hilt-android-compiler


### 添加标注
* 标注application，生成一个基础Hilt组件
	```kotlin
	@HiltAndroidApp
	class ExampleApplication : Application() { ... }
	```
* 标注android系统类，生成一个独立的Hilt组件
	```kotlin
	@AndroidEntryPoint
	class ExampleActivity : AppCompatActivity() { ...  }
	```
* 标注ViewModel类
	```kotlin
	@HiltViewModel
	class FooViewModel @Inject constructor(
		val handle: SavedStateHandle,
		val foo: Foo
	) : ViewModel
	```

#### 支持的系统类
* Application
* Activity
* Fragment
* View
* Service
* BroadcastReceiver


### 添加依赖
* 使用`Inject`添加
	```kotlin
	class ExampleActivity : AppCompatActivity() { 
		@Inject lateinit var analytics: AnalyticsAdapter
		...
	}
	```

### 绑定实例
* 使用`Inject`绑定，构造函数的参数被认为是依赖项
	```kotlin
	class AnalyticsAdapter @Inject constructor(
		private val service: AnalyticsService
	) { ... }
	```
* 使用`Binds`绑定，用于接口类
	```kotlin
	interface AnalyticsService {
		fun analyticsMethods()
	}

	class AnalyticsServiceImpl @Inject constructor(
		...
	) : AnalyticsService { ... }

	@Module    //定义一个Hilt模块
	@InstallIn(ActivityComponent::class)  //告知Hilt模块用于哪个系统类上
	abstract class AnalyticsModule {
		@Binds //返回类型告知Hilt绑定哪个接口类，参数告知Hilt要绑定的实现类
		abstract fun bindAnalyticsService(
			analyticsServiceImpl: AnalyticsServiceImpl
		): AnalyticsService
	}
	```
* 使用`Provides`绑定，用于三方库
	```kotlin
	@Module
	@InstallIn(ActivityComponent::class)
	object AnalyticsModule {

		@Provides //返回类型告知Hilt绑定哪个类型，参数告知Hilt对应的依赖项
		fun provideAnalyticsService(): AnalyticsService {
			return Retrofit.Builder()
					.baseUrl("https://example.com")
					.build()
					.create(AnalyticsService::class.java)
		}
	}
	```

### 限定符
* `Qualifier`用于给同一类型绑定多个实例

#### 示例
```kotlin
//定义标注
@Qualifier
@Retention(AnnotationRetention.BINARY)
annotation class AuthInterceptorOkHttpClient

//定义provider
@Module
@InstallIn(ApplicationComponent::class)
object NetworkModule {
	@AuthInterceptorOkHttpClient
	@Provides
	fun provideAuthInterceptorOkHttpClient(
			authInterceptor: AuthInterceptor
	): OkHttpClient {
		return OkHttpClient.Builder()
				.addInterceptor(authInterceptor)
				.build()
	}
}

//注入依赖
class ExampleServiceImpl @Inject constructor(
  @AuthInterceptorOkHttpClient private val okHttpClient: OkHttpClient
) : ...
```


### 作用域
* 用于将实例绑定到特定的系统类上
* 标注
	* `@Singleton`  绑定到Application类
	* `@ActivityScoped`  绑定到Activity类
	* `@ServiceScoped`  绑定到Service类

#### 示例
```kotlin
@ActivityScoped
class AnalyticsAdapter @Inject constructor(
	private val service: AnalyticsService
) { ... }

@Module
@InstallIn(ApplicationComponent::class)
object AnalyticsModule {
	@Singleton
	@Provides
	fun provideAnalyticsService(): AnalyticsService {
		return Retrofit.Builder()
				.baseUrl("https://example.com")
				.build()
				.create(AnalyticsService::class.java)
	}
}
```


## Retrofit2

* [参考手册](https://square.github.io/retrofit/)

### 功能
* 封装HTTP库的接口
* 将HTTP请求转换成一个接口函数，将请求、响应数据转换成对象


### 添加依赖
* 一个运行库 `com.squareup.retrofit2:retrofit`

### 标注接口
* GET请求
	`@GET("users/list?sort=desc")` 
* 带参数的GET请求
	```kotlin
	//Path为路径参数，Query为查询参数
	@GET("group/{id}/users")
	fun groupList(@Path("id") groupId: Int, 
				  @Query("sort") sort: String): Call<List<User>>


	@GET("group/{id}/users")
	fun groupList(@Path("id") groupId: Int, 
				  @QueryMap options: Map<String, String>): Call<List<User>>
	```
* 带内容的POST请求
	```kotlin
	//Body指定请求内容
	@POST("users/new")
	fun createUser(@Body user: User): Call<User>
	```
* 带静态头信息的GET请求
	```kotlin
	@Headers("Cache-Control: max-age=640000")
	@GET("widget/list")
	fun widgetList(): Call<List<Widget>>

	//多个头信息
	@Headers({
		"Accept: application/vnd.github.v3.full+json",
		"User-Agent: Retrofit-Sample-App"
	})
	@GET("users/{username}")
	fun getUser(@Path("username") String username): Call<User> 
	```
* 带动态头信息的GET请求
	```kotlin
	@GET("user")
	fun getUser(@Header("Authorization") String authorization): Call<User>

	@GET("user")
	fun getUser(@HeaderMap headers: Map<String, String>): Call<Usr>
	```

### 创建对象
```kotlin
val retrofit: Retrofit = new Retrofit.Builder()
		.baseUrl("https://api.github.com/")
		.addConverterFactory(GsonConverterFactory.create())
		.build()

val service: GitHubService = retrofit.create(GitHubService.class)
```


### 执行请求
