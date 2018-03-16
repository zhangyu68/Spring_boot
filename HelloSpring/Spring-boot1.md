# Spring boot 快速入门
## Spring boot 项目结构介绍
Spring Boot的基础结构共三个文件:
- src/main/java 程序开发以及主程序入口
- src/main/resources 配置文件
- src/test/java 测试程序
### 建议目录如下
root package结构：==com.example.myproject==

```
com
  +- example
    +- myproject
      +- Application.java
      |
      +- domain
      |  +- Customer.java
      |  +- CustomerRepository.java
      |
      +- service
      |  +- CustomerService.java
      |
      +- controller
      |  +- CustomerController.java
      |
```

- Application.java建议放在根节点目录下，主要做一些框架配置
- domain目录主要用于实体(Entity)与数据访问层（Respository）
- service 层主要是业务类代码
- controller 负责页面访问控制

#### 引入web模块
1、pom.xml中添加支持web的模块
```
<dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
 </dependency>
```

pom.xml文件中默认有两个模块：
- spring-boot-starter ：核心模块，包括自动配置支持、日志和YAML；
- spring-boot-starter-test ：测试模块，包括JUnit、Hamcrest、Mockito。
- 

2、编写controller内容：

```
@RestController
public class HelloWorldController {
    @RequestMapping("/hello")
    public String index() {
        return "Hello World";
    }
}
```

@RestController 意味着controller里面的方法都以json格式输出，不用再写什么jackjson配置的了！

3、启动主程序，打开浏览器访问http://localhost:8080/hello，就可以看到效果

#### 如何做单元测试
单元测试主要用到的是Mockmvc。

打开的src/test/下的测试入口，编写简单的http请求来测试；使用mockmvc进行，利用MockMvcResultHandlers.print()打印出执行结果。

```
@RunWith(SpringRunner.class)
@SpringBootTest
public class HelloTests {

  
    private MockMvc mvc;

    @Before
    public void setUp() throws Exception {
        mvc = MockMvcBuilders.standaloneSetup(new HelloWorldController()).build();
    }

    @Test
    public void getHello() throws Exception {
        mvc.perform(MockMvcRequestBuilders.get("/hello").accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(content().string(equalTo("Hello World")));
    }

}
```

#### 开发环境的调试
项目的热启动，添加配置

```
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <fork>true</fork>
            </configuration>
        </plugin>
</plugins>
</build>
```


