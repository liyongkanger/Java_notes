## MybatisPlus

pom

```xml
         <!--mybatis-plus 持久层-->
         <!--
          <mybatis-plus.version>3.0.5</mybatis-plus.version> 
          <velocity.version>2.0</velocity.version> -->
            <dependency>
                <groupId>com.baomidou</groupId>
                <artifactId>mybatis-plus-boot-starter</artifactId>
                <version>${mybatis-plus.version}</version>
            </dependency>

            <!-- velocity 模板引擎, Mybatis Plus 代码生成器需要 -->
            <dependency>
                <groupId>org.apache.velocity</groupId>
                <artifactId>velocity-engine-core</artifactId>
                <version>${velocity.version}</version>
            </dependency>
```



### application.properties

```properties
spring.datasource.username=root
spring.datasource.password=admin
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis_plus?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC&allowPublicKeyRetrieval=true
###  mysql 8  的驱动  com.mysql.cj.jdbc.Driver 兼容 5
#    mysql 5  驱动    com.mysql.jdbc.Driver
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
# 配置日志
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl

# 配置逻辑删除
mybatis-plus.global-config.db-config.logic-delete-value=1
mybatis-plus.global-config.db-config.logic-not-delete-value=0

###设置开发环境 dev
spring.profiles.active=dev
```

### config配置类

```java
@Configuration
@EnableTransansactionManagement
@MapperScan("com.lyk.mapper")
public class MybatisPlusConfig{
    //注册乐观锁插件
    @Bean
    public OptimisticLockerInterceptor optimisticLockerInterceptor(){
        return new OptimisticLockerInterceptor();
    }
    
    //分页插件
    @Bean 
    public PaginationInterceptor paginationInterceptor(){
        return new PaginationInterceptor();
    }
    //逻辑删除组件
    public IsqlInjector sqlInjector(){
        return new LogicSqlInjector();
    }
    //性能分析插件
    @Bean 
    @Profile({"div", "test"})
    public PerformanceInterceptor performanceInterceptor(){
         PerformanceInterceptor performanceInterceptor = new PerformanceInterceptor();
        performanceInterceptor.setMaxTime(100); //设置sql最大时间
        performanceInterceptor.setFormat(true); // sql格式化
        return performanceInterceptor ;
    }
}
```

### 设置自动填充

```java
@Slf4j
@Component
public class MyMetaObjectHandler implements MetaObjectHandler{
    public void insertFill(MetaObject metaObject){
        log.info("start insert fill.....");
        this.setFieldValByName("createTime", new Date(), metaObject);
        this.setFieldValByName("modifiedTime", new Date(), metaObject);
        
    }
    @Override
    public void updateFill(MetaObject metaObject) {
        log.info("start update fill.....");
        this.setFieldValByName("modifiedTime",new Date(),metaObject);
    }
}
```

### 代码自动生成

```java
@Test
    public void run() {

        // 1、创建代码生成器
        AutoGenerator mpg = new AutoGenerator();

        // 2、全局配置
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        gc.setOutputDir("E:\\work\\guli_parent\\service\\service_edu" + "/src/main/java");

        gc.setAuthor("testjava");
        gc.setOpen(false); //生成后是否打开资源管理器
        gc.setFileOverride(false); //重新生成时文件是否覆盖

        //UserServie
        gc.setServiceName("%sService");	//去掉Service接口的首字母I

        gc.setIdType(IdType.ID_WORKER_STR); //主键策略
        gc.setDateType(DateType.ONLY_DATE);//定义生成的实体类中日期类型
        gc.setSwagger2(true);//开启Swagger2模式

        mpg.setGlobalConfig(gc);

        // 3、数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/guli?serverTimezone=GMT%2B8");
        dsc.setDriverName("com.mysql.cj.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("root");
        dsc.setDbType(DbType.MYSQL);
        mpg.setDataSource(dsc);

        // 4、包配置
        PackageConfig pc = new PackageConfig();
        pc.setModuleName("eduservice"); //模块名
        //包  com.atguigu.eduservice
        pc.setParent("com.atguigu");
        //包  com.atguigu.eduservice.controller
        pc.setController("controller");
        pc.setEntity("entity");
        pc.setService("service");
        pc.setMapper("mapper");
        mpg.setPackageInfo(pc);

        // 5、策略配置
        StrategyConfig strategy = new StrategyConfig();

        strategy.setInclude("edu_course","edu_course_description","edu_chapter","edu_video");

        strategy.setNaming(NamingStrategy.underline_to_camel);//数据库表映射到实体的命名策略
        strategy.setTablePrefix(pc.getModuleName() + "_"); //生成实体时去掉表前缀

        strategy.setColumnNaming(NamingStrategy.underline_to_camel);//数据库表字段映射到实体的命名策略
        strategy.setEntityLombokModel(true); // lombok 模型 @Accessors(chain = true) setter链式操作

        strategy.setRestControllerStyle(true); //restful api风格控制器
        strategy.setControllerMappingHyphenStyle(true); //url中驼峰转连字符

        mpg.setStrategy(strategy);


        // 6、执行
        mpg.execute();
    }
```

### api

#### 条件查询

​       非空查询         //查询 name 不为空 并且邮箱不为空（wrapper 是封装复杂参数）

```java
private UserMapper userMapper;

void warppertest(){
    //查询 name 不为空 并且邮箱不为空
    QueryMapper<User> warapper = new QueryWrapper<>();
    wrapper 
        .isNotNUll("name")
        .isNotNUll("email")
        .ge("age",12) // get equal
        userMapper.selectList(wrapper).forEach(System.out::println);
}
```

​      单个查询     查询名字等于 kk

```java 
public void testWrapper(){
    //查询名字等于kk 
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.eq("name", "kk");
    User user = userMapper.selectOne(wrapper);// 查询一个用One 多个用list 或者map
    System.out.println(user);
}
```

​    查询区间

```java
public void testWrapperl(){
QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.between("age", 20, 20);
    userMapper.selectCount(wrapper);
}
```

模糊查询

```java 
public void test(){
    //模糊查询
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper
        .notLike("name", "e")
        .likeRight("email", "t");
    List<Map<String,Object>> maps = userMapper.selectMaps(wrapper);
    maps.forEach(System.out::println);
}
```

内查询

```java
public void test(){
    //id 在子查询里查询出来
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.inSql("id"，"select id from usr where id < 3");
    List<Object> objects = userMapper.selectObjs(wrapper);
    objects.forEach(System.out::println);
}
```

 升序降序

```java 
public void test6(){
    //降序升序
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.orderByDesc("id");// 降序desc  升序ASc
    List<User> users = userMapper.selectList(wrapper);
    users.forEach(System.out::println);
}
```

---

#### 简单crud：

查询所有

```java 
@AutoWire
private UserMapper usermapper; // 继承了BaseMapper方法来自父类

void contentLoads(){
    //参数是一个Wrapper条件构造器， 不用就填null
    //查询所有用户
    List<User> users = userMapper.selectList(null);
    users.forEach(System.out::println);
}
```

插入

```java
public void insert(){
    User user = new User();
    user.setName("kk");
    user.setAge(3);
    user.setEmail("745519261@qq.com");
    int result = userMapper(user);
    System.out.println(result); // 受影响的行数
    System.out.println(user);// 增加一个id id 是全局默认唯一id
}
```

  修改(乐观锁)

```java
public void testOptimisticLocker(){
    //`1 查询用户信息
    User user = userMapper.selectById(1L);
    //2 修改用户信息
    user.setName("hh");
    user.setEmail("13234");
    //3. 执行
    userMapper.updateById(user);
}
```

乐观锁失败

```java
public void testOptimisticLocker(){
    //线程1 
    //查询
    User user = userMapper.selectById(1L);
    //2 修改
     user.setName("hh");
        user.setEmail("12344");
    //模拟另外一个线程插队操作
        User user2 =  userMapper.selectById(1L);
        user2.setName("222");
        user2.setEmail("222");
      //3.执行更新
        userMapper.updateById(user2);

        userMapper.updateById(user); // 如果没有乐观锁就会覆盖插队线程的值
}
```

查询

```java
public void testSelectById(){
    User user = userMapper.selectById(1L);
    System.out.println(user);
}
```

查询多个

```java
public void testSelectById(){
    List<User> users = userMapper.selectBatchIds(Arrays.asList(1,2,3));
    users.forEach(System.out.prinlnt;
}
```

条件查询map

```java
public void testSelectByBatchIds(){
   HashMap<String, Object> map = new HashMap<>();
    map.put("name", "kk");
    //多条件查询
    List<User> users = userMapper.selectByMap(map);
    users.forEach(System.out::println);
}
```

分页查询

```java
public void testPage(){
    // 参数1 当前页
    // 参数2 页面大小
    // 使用分页查询 分页简单 使用mp的Page     cur1  size5
    Page<User> page = new Page<>(1,5);
    System.out.println(page.getTotal());
    userMapper.selectPage(page,null);
    page.getRecords().forEach(System.out::println); 
}
```

删除

```java
public void testDeletedById(){
    userMapper.deleteById(1L);
}
```

批量删除

```java
public void testDeleteById(){
    userMapper.deleteBatchIds(Arrays.asList(3,4,5));
}
```

通过map删除

```java
public void testDeleteMap(){
    HashMap<String, Object> map = new HashMap<>();
    map.put("name", "kk");
    userMapper.deleteByMap(map);
}
```

### 实体类注解

```
      //雪花算法 对应生成数据库中的主键
      @TableId(type = IdType.AUTO)  // 默认全局唯一Id
      private Long id;  //数据库字段要加 auto

      private String name;
      private Integer age;
      private String email;
      // 字段添加内容
      @TableField(fill = FieldFill.INSERT)
      private Date  createTime;
      @TableField(fill = FieldFill.INSERT_UPDATE)
      private Date  modifiedTime;
      @Version  //乐观锁
      private Integer version;
      @TableLogic  // 逻辑删除
      private Integer deleted;

```





