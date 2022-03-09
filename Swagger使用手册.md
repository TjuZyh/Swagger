### Swagger使用手册

------

#### 一 简介

**前后端时代：**

- 前端：前端控制器、视图层
- 后端：控制层controller、服务层service、数据访问层dao
- 前后端交互：通过API接口
- 前后端相对独立，松耦合，可以部署在不同的服务器上

**前后端联调问题：可以使用swagger，实时更新API文档，方便测试**

#### 二 springboot集成Swagger

##### 1. 导入Swagger依赖

**spring-boot-starter-parent 版本号为：2.5.4**

```xml
				<!--swagger所需的依赖-->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
```

##### 2. 编写Swagger配置类

**在项目主程序同级的目录下创建config包，在其中创建SwaggerConfig配置类**

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket webApiConfig(){
        return new Docket(DocumentationType.SWAGGER_2)
                //设置组名
                .groupName("webApi")
                .apiInfo(webApiInfo())
                //配置是否启动swagger，默认为ture
                //如果项目正式上线，则不可以访问swagger
                .enable(true)
                .select()
                /**
                 * apis():指定扫描的接口
                 *  RequestHandlerSelectors:配置要扫描接口的方式
                 *       basePackage:指定要扫描的包
                 *       any:扫面全部
                 *       none:不扫描
                 *       withClassAnnotation:扫描类上的注解(参数是类上注解的class对象)
                 *       withMethodAnnotation:扫描方法上的注解(参数是方法上的注解的class对象)
                 */
                .apis(RequestHandlerSelectors.basePackage("com.zyh.credit.controller"))
                /**
                 * paths():过滤路径
                 *  PathSelectors:配置过滤的路径
                 *      any:过滤全部路径
                 *      none:不过滤路径
                 *      ant:过滤指定路径:按照按照Spring的AntPathMatcher提供的match方法进行匹配
                 *      regex:过滤指定路径:按照String的matches方法进行匹配
                 */
                /*.paths(PathSelectors.ant("/zyh/credit/**"))*/
                .build();

    }

    private ApiInfo webApiInfo(){

        return new ApiInfoBuilder()
                .title("积分商城Restful Apis 详细说明文档")
                .description("说明积分商城的Apis")
                .version("1.0")
                .contact(new Contact("Tju_Zyh" , "https://github.com/TjuZyh" , "tju_zhaoyihan@163.com"))
                .build();
    }

}
```

**便捷开发版：**

```java
		@Bean
    public Docket webApiConfig(){
        return new Docket(DocumentationType.SWAGGER_2)
                .groupName("webApi")
                .apiInfo(webApiInfo())
                .enable(true)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.zyh.credit.controller"))
                .build();

    }

    private ApiInfo webApiInfo(){

        return new ApiInfoBuilder()
                .title("积分商城Restful Apis 详细说明文档")
                .description("说明积分商城的Apis")
                .version("1.0")
                .contact(new Contact("Tju_Zyh" , "https://github.com/TjuZyh" , "tju_zhaoyihan@163.com"))
                .build();
    }
```

##### 3. 使用Swagger

**通过访问：localhost:8080/swagger-ui.html访问（端口号为项目端口号）**

##### 4. 常用注解

**I. 在实体类上和其属性上添加注解来添加对应的注释**

- `@ApiModel` : 为类添加注释
- `@ApiModelProperty` ：为类的属性添加注解

```java
@Data
@EqualsAndHashCode
@ApiModel("用户实体")
public class User {
    @ApiModelProperty(value = "用户ID，数据库自增")
    private long userId;

    @ApiModelProperty(value = "用户账号")
    private String account;

    @ApiModelProperty(value = "用户性别 男1女0")
    private Integer gender;

    @ApiModelProperty(value = "用户昵称")
    private String nickName;

    @ApiModelProperty(value = "用户真实姓名")
    private String realName;

    @ApiModelProperty(value = "用户手机号")
    private String phone;

    @ApiModelProperty(value = "用户生日")
    private String birthday;

    @ApiModelProperty(value = "用户余额")
    private double balance;
}
```

**II. 在控制类及其方法上添加相应注解**

- `@Api` ：为控制类添加说明
- `@ApiOperation` : 为控制类的接口方法添加注解说明

```java
@Api(tags = "用户相关接口")
@RestController
public class UserController {

    @ApiOperation("获取用户信息")
    @GetMapping("hello")
    public User getUser(){
        return new User();
    }

}
```

