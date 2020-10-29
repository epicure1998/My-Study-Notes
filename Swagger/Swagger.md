# Swagger

前后端分离：Vue+SpringBoot

前端伪造Json数据，前端给后端提供数据类型，后端写接口

RestFul Api文档在线自动生成工具=>Api文档与API定义同步更新

可以在线测试API接口

在正式开发时记得关闭

## 使用：

### 导包

 ```xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>

 ```

```xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>

```



### 写配置

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket docket1(){
        return new Docket(DocumentationType.SWAGGER_2).groupName("AA");
    }


    @Bean
    public Docket docket(){

        //Profiles profiles = Profiles.of();

        return new Docket(DocumentationType.SWAGGER_2)
                    .apiInfo(apiInfo())
                    .select()
                    //basePackage指定包扫描
                    //withClassAnnotation:扫描类上的注解
                    //paths过滤不扫描
                    .apis(RequestHandlerSelectors.basePackage("com.qiumengke.swagger.controller"))
                    .build()
                    .enable(true);//是否启动Swagger
    }

    public ApiInfo apiInfo(){
        Contact contact = new Contact("Travis","pingguyitailang.top","pingguyitailang@126.com");

        return new ApiInfo(
                "Travis's swager",
                "it's lit",
                "1.8",
                "pingguyitailang.top",
                "Travis Scott",
                "Apache 2.0",
                "www.baidu.com");
    }
}
```

默认的配置文件

### 测试

#### 如何配置多个组

