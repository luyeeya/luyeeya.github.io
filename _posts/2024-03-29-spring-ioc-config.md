---
layout: post
title: Spring IoC 配置方式
---

# Spring IoC 配置方式

> 当前主流 Java 开发中一般用：注解配置 + Java Config

### 1⃣️ XML 配置

将 Bean 的信息配置在 xml 文件中，然后由 Spring 加载解析后创建 Bean

```xml
<!-- xml 配置 -->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd"
>
    <bean id="userMapper" class="spring.bean.UserMapper" />
    <bean id="userService" class="spring.bean.UserServiceImpl">
        <property name="userMapper" ref="userMapper" />
    </bean>
</beans>
```

```java
/**
 * Spring IoC 配置之：xml 配置
 */
public class XmlConfig {
    private static final String XML_PATH = "xml_config.xml";
    public static void main(String[] args) {
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext(XML_PATH); // ClassPathXmlApplicationContext
        UserServiceImpl userService = (UserServiceImpl) applicationContext.getBean("userService");
        System.out.println(userService.listUser());
    }
}
```

优点：修改配置不需重新编译

缺点：配置繁琐，扩展性差

### 2⃣️ Java Config

使用一个类专门配置 Bean 的信息

```java
/**
 * Spring IoC 配置之：Java Config 配置
 */
@Configuration // 添加 @Configuration 注解声明为配置类
public class JavaConfig {
    @Bean(name = "userMapper") // Spring 会管理 @Bean 注解修饰的方法返回的 Bean 实例
    public UserMapper userMapper() {
        return new UserMapper();
    }

    @Bean(name = "userService")
    public UserServiceImpl userService(UserMapper userMapper) { // Autowiring by type from bean name 'userService' via factory method to bean named 'userMapper'
        UserServiceImpl userService = new UserServiceImpl();
        userService.setUserMapper(userMapper); // 手动设置 userMapper 属性
        return userService;
    }

    public static void main(String[] args) {
        AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(JavaConfig.class); // AnnotationConfigApplicationContext
        UserServiceImpl userService = (UserServiceImpl) applicationContext.getBean("userService");
        System.out.println(userService.listUser());
    }
}
```

优点：配置方便，扩展性高

缺点：修改配置需要重新编译，可读性较差

### 3⃣️ 注解配置

通过在类上加注解声明交给 Spring 管理

配置注解扫描器后，Spring 会扫描带 @Component、@Controller、@Service、@Repository 等注解的类，然后创建并管理这些 Bean

- 配置注解扫描器的两种方式
    1. 使用 xml 配置：`<context:component-scan base-package='spring.bean'>`
    2. 使用注解配置：`@ComponentScan("spring.bean")`

```java
@Repository // 注解声明
public class UserMapper {
    public List<User> listUser() {
        User user1 = new User("Tom");
        User user2 = new User("Jerry");
        User user3 = new User("ZhangSan");
        return Arrays.asList(user1, user2, user3);
    }
}

@Service("userService") // 注解声明
public class UserServiceImpl {
    private UserMapper userMapper;

    @Autowired
    public void setUserMapper(UserMapper userMapper) {
        this.userMapper = userMapper;
    }

    public List<User> listUser() {
        return userMapper.listUser();
    }
}
```

优点：配置方便，通俗易懂

缺点：无法给第三方 jar 包的类上加上注解
