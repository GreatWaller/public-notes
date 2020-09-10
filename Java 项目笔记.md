# Java 项目学习笔记

### 1 dubbo

- 实现类@Service(interfaceClass = AppSignRecordDetailService.class, version = "1.0.0")

- ```java
  @Reference(version = "1.0.0")
  private AppSignRecordDetailService appSignRecordDetailService;
  ```

  - 注意版本号需要匹配

  - 不需要再@Autowired

### 2 druid

##### 2.1 多datasource

详请参考 https://blog.csdn.net/mcccmay/article/details/80245983

```xml
<!--druid多数据源-->
<dependency>
   <groupId>com.alibaba</groupId>
   <artifactId>druid-spring-boot-starter</artifactId>
   <version>1.1.10</version>
</dependency>
```



- datasourceAspect

- DataSourceNames

- DynamicDataSource

- DynamicDataSourceConfig

- ```java
  @SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
  ```



### 3 Controller

##### 3.1 一个标准示例

```java
    @RequestMapping(value = "/getMeetingInfo", method = RequestMethod.GET)
    public ResponseEntity<?> getMeetingInfo(@RequestParam(value = "uuid", required = true) String uuid) {
        Map resultMap = appCompareService.getCurrentMeeting(uuid);
        return ResponseEntity.ok(resultMap);
    }
```

### 4 通用知识点

#### 4.1 @Bean

典型用法

```java
@Configuration
public class AppConfig {

  	// 使用@Bean 注解表明myBean需要交给Spring进行管理
  	// 未指定bean 的名称，默认采用的是 "方法名" + "首字母小写"的配置方式
    @Bean
    public MyBean myBean(){
        return new MyBean();
    }
}
```

与其它注解联合使用

- @Profile：通过环境变量选择
- Scope

```java
@Configuration
public class AppConfigWithAliasAndScope {

    /**
     * 为myBean起两个名字，b1 和 b2
     * @Scope 默认为 singleton，但是可以指定其作用域
     * prototype 是多例的，即每一次调用都会生成一个新的实例。
     */
    @Bean({"b1","b2"})
    @Scope("prototype")
    public MyBean myBean(){
        return new MyBean();
    }
}
```

- @Lazy 作用有类上
- @DependsOn

```java
@Configuration
public class AppConfigWithDependsOn {

    @Bean("firstBean")
    @DependsOn(value = {
            "secondBean",
            "thirdBean"
    })
    public FirstBean firstBean() {
        return new FirstBean();
    }

    @Bean("secondBean")
    public SecondBean secondBean() {
        return new SecondBean();
    }

    @Bean("thirdBean")
    public ThirdBean thirdBean() {
        return new ThirdBean();
    }
}
```

- @Primary：有候选时优先考虑

> 注意： 除非使用component-scanning进行组件扫描，否则在类级别上使用@Primary不会有作用。

```java
@Configuration
public class AppConfigWithPrimary {

    @Bean
    public MyBean myBeanOne(){
        return new MyBean();
    }

    @Bean
    @Primary
    public MyBean myBeanTwo(){
        return new MyBean();
    }
}
```

#### 4.2 @ComponentScan

> 如果你的其他包都在使用了@SpringBootApplication注解的mainapp所在的包及其下级包，则你什么都不用做，SpringBoot会自动帮你把其他包都扫描了
>
> 如果你有一些bean所在的包，不在mainapp的包及其下级包，那么你需要手动加上@ComponentScan注解并指定那个bean所在的包

- **自定扫描路径下边带有@Controller，@Service，@Repository，@Component注解加入spring容器**
- **通过includeFilters加入扫描路径下没有以上注解的类加入spring容器**
- **通过excludeFilters过滤出不用加入spring容器的类**

```java
@ComponentScan({"com.demo.springboot","com.demo.somethingelse"})
@SpringBootApplication
public class SpringbootApplication {
特别注意一下：如果仅仅只写@ComponentScan({"com.demo.somethingelse"})将导致com.demo.springboot包下的类无法被扫描到（框架原始的默认扫描效果无效了）

### 在某个类上使用@Component注解，表明当需要创建类时，这个被注解的类是一个候选类。就像是举手。
### @ComponentScan用于扫描指定包下的类。就像看都有哪些举手了。
```



#### 5 Mybatis-Plus

![img](https://upload-images.jianshu.io/upload_images/14210893-af17899efaa0a18f?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

示例

```java
public void selectList() {
        List<User> list = mapper.selectList(null);

        System.out.println(list);
    }

public void selectPage() {
        Page<User> page = new Page<>(1, 5);
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();

        IPage<User> userIPage = mapper.selectPage(page, queryWrapper);
        System.out.println(userIPage);
    }

public void delete() {
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper
                .isNull("name")
                .ge("age", 12)
                .isNotNull("email");
        int delete = mapper.delete(queryWrapper);
        System.out.println("delete return count = " + delete);
    }

public void update() {

        //修改值
        User user = new User();
        user.setStatus(true);
        user.setName("zhangsan");

        //修改条件s
        UpdateWrapper<User> userUpdateWrapper = new UpdateWrapper<>();
        userUpdateWrapper.eq("name", "lqf");

        int update = mapper.update(user, userUpdateWrapper);

        System.out.println(update);
    }
```

### 5 需要注意的细节

#### 5.1 序列化

> 实体及DTO 需要 implements Serializable

#### 5.2 Long

```
Failed to convert value of type 'java.lang.String' to required type 'java.lang.Long'; nested exception is java.lang.NumberFormatException: For input string: "30349736526780152801"
```

#### 5.3 Select Distinct



## 6 Lombok

#### 6.1 `@Getter/@Setter`注解

可以针对类的属性字段自动生成Get/Set方法。

```java
public class OrderCreateDemoReq{

    @Getter
    @Setter
    private String customerId;

    @Setter
    @Getter
    private String poolId;
    
    //其他代码……
}

//上面请求Req类的代码相当于如下：

public class OrderCreateDemoReq{
    private String customerId;    
    private String poolId;

    public String getCustomerId(){
         return customerId;
    }

    public String getPoolId(){
         return poolId;
    }

    public void setCustomerId(String customerId){
         this.customerId = customerId;
    }

    public void setPoolId(String poolId){
         this.pool = pool;
    }

}
```

#### 6.2 `@ToString`注解

为使用该注解的类生成一个toString方法，默认的toString格式为：ClassName(fieldName= fieleValue ,fieldName1=fieleValue)。

```java
@ToString(callSuper=true,exclude="someExcludedField")
public   class Demo extends Bar {

    private boolean someBoolean = true;
    private String someStringField;
    private float someExcludedField;

}

//上面代码相当于如下：

public   class Demo extends Bar {

    private boolean someBoolean = true;
    private String someStringField;
    private float someExcludedField;
   
    @ Override
    public String toString() {
        return "Foo(super=" +   super.toString() +
            ", someBoolean=" +   someBoolean +
            ", someStringField=" +   someStringField + ")";
    }
}
```

#### 6.3 `@EqualsAndHashCode`注解

为使用该注解的类自动生成equals和hashCode方法。

```java
@EqualsAndHashCode(exclude = {"id"}, callSuper =true)
public class LombokDemo extends Demo{
    private int id;
    private String name;
    private String gender;
}

//上面代码相当于如下：

public class LombokDemo extends Demo{

    private int id;
    private String name;
    private String gender;
    
    @Override
    public boolean equals(final Object o) {
        if (o == this) return true;
        if (o == null) return false;
        if (o.getClass() != this.getClass()) return false;
        if (!super.equals(o)) return false;
        final LombokDemo other = (LombokDemo)o;
        if (this.name == null ? other.name != null : !this.name.equals(other.name)) return false;
        if (this.gender == null ? other.gender != null : !this.gender.equals(other.gender)) return false;
        return true;
    }

    @Override
    public int hashCode() {
        final int PRIME = 31;
        int result = 1;
        result = result * PRIME + super.hashCode();
        result = result * PRIME + (this.name == null ? 0 : this.name.hashCode());
        result = result * PRIME + (this.gender == null ? 0 : this.gender.hashCode());
        return result;
    }

}
```

#### 6.4 @NoArgsConstructor`, `@RequiredArgsConstructor`, `@AllArgsConstructor

这几个注解分别为类自动生成了无参构造器、指定参数的构造器和包含所有参数的构造器。

```java
@RequiredArgsConstructor(staticName = "of") 
@AllArgsConstructor(access = AccessLevel.PROTECTED) 
public class ConstructorExample<T> { 

  private int x, y; 
  @NonNull private T description; 
  
  @NoArgsConstructor 
  public static class NoArgsExample { 
    @NonNull private String field; 
  } 

}

//上面代码相当于如下：
public class ConstructorExample<T> { 
  private int x, y; 
  @NonNull private T description; 

  private ConstructorExample(T description) { 
    if (description == null) throw new NullPointerException("description"); 
    this.description = description; 
  } 

  public static <T> ConstructorExample<T> of(T description) { 
    return new ConstructorExample<T>(description); 
  } 

  @java.beans.ConstructorProperties({"x", "y", "description"}) 
  protected ConstructorExample(int x, int y, T description) { 
    if (description == null) throw new NullPointerException("description"); 
    this.x = x; 
    this.y = y; 
    this.description = description; 
  } 
  
  public static class NoArgsExample { 
    @NonNull private String field;
    
    public NoArgsExample() { 
    } 
  } 
}
```

#### 6.4 @Data

其包含注解的集合`@ToString`，`@EqualsAndHashCode`，所有字段的`@Getter`和所有非final字段的`@Setter`, `@RequiredArgsConstructor`。

#### 6.5 @Builder

提供了一种比较推崇的构建值对象的方式。

```java
@Builder 
public class BuilderExample { 

  private String name; 
  private int age; 

  @Singular private Set<String> occupations; 

}

//上面代码相当于如下：

public class BuilderExample { 

  private String name; 
  private int age; 
  private Set<String> occupations; 

  BuilderExample(String name, int age, Set<String> occupations) { 
    this.name = name; 
    this.age = age; 
    this.occupations = occupations; 
  } 

  public static BuilderExampleBuilder builder() { 
    return new BuilderExampleBuilder(); 
  } 

  public static class BuilderExampleBuilder { 

    private String name; 
    private int age; 
    private java.util.ArrayList<String> occupations;    

    BuilderExampleBuilder() { 
    } 

    public BuilderExampleBuilder name(String name) { 
      this.name = name; 
      return this; 
    } 

    public BuilderExampleBuilder age(int age) { 
      this.age = age; 
      return this; 
    } 

    public BuilderExampleBuilder occupation(String occupation) { 
      if (this.occupations == null) { 
        this.occupations = new java.util.ArrayList<String>(); 
      } 
      this.occupations.add(occupation); 
      return this; 
    } 

    public BuilderExampleBuilder occupations(Collection<? extends String> occupations) { 
      if (this.occupations == null) { 
        this.occupations = new java.util.ArrayList<String>(); 
      } 
      this.occupations.addAll(occupations); 
      return this; 
    } 

    public BuilderExampleBuilder clearOccupations() { 
      if (this.occupations != null) { 
        this.occupations.clear(); 
      }
      return this; 
    } 

    public BuilderExample build() {  
      Set<String> occupations = new HashSet<>(); 
      return new BuilderExample(name, age, occupations); 
    } 

    @override
    public String toString() { 
      return "BuilderExample.BuilderExampleBuilder(name = " + this.name + ", age = " + this.age + ", occupations = " + this.occupations + ")"; 
    } 
  } 
}
```

#### 6.6 @Synchronized

类似Java中的Synchronized 关键字，但是可以隐藏同步锁。

```java
public class SynchronizedExample { 

 private final Object readLock = new   Object(); 

 @Synchronized 
 public static void hello() { 
     System.out.println("world");   
 } 

 @Synchronized("readLock") 
 public void foo() { 
   System.out.println("bar"); 
 } 

//上面代码相当于如下：

 public class SynchronizedExample { 

  private static final Object $LOCK = new   Object[0]; 
  private final Object readLock = new   Object(); 

  public static void hello() { 
    synchronized($LOCK) { 
      System.out.println("world"); 
    } 
  }   

  public void foo() { 
    synchronized(readLock) { 
        System.out.println("bar");   
    } 
  } 

}
```

<img src="Java 项目笔记.assets/v2-6e090d13253b9fc4371fb6094fcaea86_720w.jpg" alt="img"  />

<img src="Java 项目笔记.assets/v2-6a1bd958d0a800b9bc1c13f88e498fd5_720w.jpg" alt="img"  />