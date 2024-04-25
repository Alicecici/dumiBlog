---
nav: 
  title: 基础知识
  order: 0
  second:
    title: 后端
    order: 1
---
# 后端部分
## 1.跨域
需要回答的问题1：什么是跨域？ 需要回答的问题2：跨域的解决方式有哪些？

跨域是指同一个服务器上的两个不同URL之间的通信。域名 mismatch（域名不匹配）是指请求的URL和响应的URL之间的域名不匹配。这种情况下，浏览器会阻止跨域通信，以防止潜在的攻击。

解决跨域问题有两种方法：

1. 使用CORS（跨域资源共享）：CORS是一个HTTP头，它可以用于配置服务器，允许特定域名的请求。这是解决跨域问题的一种广泛使用的方法。
2. 使用中间件（Middleware）：在应用程序中，可以使用中间件来处理跨域请求。例如，在Express中，可以使用`cors`模块来处理跨域问题。

## 2.密码加密

### 1. 了解为什么密码不能明文传输

密码不能明文传输是因为明文传输存在以下安全风险：

1. 窃听攻击：在网络传输过程中，如果密码以明文形式传输，黑客可以使用网络嗅探工具拦截和窃取密码，从而获取用户的登录凭据。

2. 中间人攻击：黑客可以在用户与服务器之间插入恶意代码或者代理服务器，窃取明文密码，这种攻击方式被称为中间人攻击。

3. 数据泄露：如果网站的数据库被攻破，存储在数据库中的明文密码会被黑客轻易获取，导致用户的账户被盗。

为了避免这些安全风险，通常会采用加密技术对密码进行加密传输，比如使用SSL/TLS协议进行加密通信，或者在前端对密码进行哈希处理再传输。这样即使黑客截获了传输的数据，也无法直接获取到用户的明文密码。

### 2. 了解前后端的加密方法

在web开发中，前后端的加密通常指的是前端对用户输入的敏感信息（比如密码）进行加密后再传输给后端服务器的过程。

前端加密：前端通常使用JavaScript等前端编程语言对用户输入的密码等敏感信息进行加密处理，比如对密码进行哈希处理。这样可以在用户输入密码后就在客户端对密码进行加密，避免明文密码在网络上传输时被窃取。

后端加密：后端服务器接收到前端传输的加密数据后，会再次对数据进行解密或者验证。后端通常会使用一些加密算法来对数据进行解密或者验证，确保数据的完整性和安全性。

通过前后端的加密，可以有效地保护用户的敏感信息不被窃取或者篡改，提高系统的安全性。常见的加密算法包括对称加密算法（如AES）、非对称加密算法（如RSA）、哈希算法（如MD5、SHA-256）等。
## 3.JWT

### 什么是JWT?
JWT（JSON Web Token）是一种用于身份验证的令牌，它可以在客户端之间安全地传输。它由一个头部（header）、一个 Payload（body） 和一个签名（signature）组成。当客户端向服务器请求授权时，服务器会生成一个JWT，并将其发送回客户端。客户端收到授权后，可以在之后的请求中携带该JWT。服务器会检查客户端提供的JWT是否有效，以确定是否授权访问特定资源。

### jWT两种使用场景：

`授权`：这是使用 JWT 的最常见的使用场景。用户登录后，每个后续请求都将包含 JWT，允许用户访问使用该令牌允许的路由、服务和资源。单点登录是当今广泛使用 JWT 的一项功能，因为它的开销很小，并且能够跨不同域轻松使用。

`信息交换`：JWT是在各方之间安全传输信息的比较便捷的方式。由于 JWT 可以签名（例如，使用公钥/私钥对），因此可以确定发送者是否是在您的授权范围之内。并且，由于签名是使用标头和有效负载计算的，因此还可以验证内容是否未被篡改。

### 后端JWT的生成

1. 生成jwtToken： 首先，代码创建了一个HashMap来存储用户ID和过期时间。然后，使用JWTUtil工具类中的createToken方法生成jwt token。这个方法接收一个Map和一个密钥（这里使用一个简单的字符串"1234"作为密钥）。最后，将生成的token设置回用户对象。
2. current： 这是一个处理当前用户请求的方法。从请求头中获取Authorization头，然后使用JWTUtil中的parseToken方法解析token。如果解析成功，获取token中的用户ID，并使用用户ID查询数据库获取用户信息。

### 应用

前端，根据jwt来调用接口，获取数据

根据token来决定是否调用接口

- 请求头中包含两个属性：`Content-Type`（设置为`application/json`）和`Authorization`（从`sessionStorage`中获取JWT令牌）。
- 请求体是一个空对象`{}`。
- 请求成功后，将返回的`jwtToken`（令牌）缓存到`sessionStorage`中。
- 响应数据存储在`data.data`中，并返回该数据。

## 4.Mybatis配置XML存放的位置
Mybatis-Plus 是一个 Mybatis 的增强工具，它简化了 Mybatis 的使用并增加了一些功能。在使用 Mybatis 或 Mybatis-Plus 时，通常需要编写
mapper.xml 文件来定义 SQL 映射。配置 XML 文件的位置是一个常见需求，以便 Mybatis-Plus 能够正确地找到并加载这些文件。

默认情况下，Mybatis-Plus 会在类路径下的 `mapper` 目录中查找 XML 映射文件。但是，你可以通过配置来修改这个默认行为，指定 XML
文件的具体位置。以下是如何在 Spring Boot 应用中配置 XML 文件位置的步骤：

### 1. 在 `application.properties` 或 `application.yml` 中配置

如果你使用的是 Spring Boot，可以在 `application.properties` 或 `application.yml` 配置文件中指定 mapper 文件的位置。

**application.properties 示例：**

```properties
mybatis-plus.mapper-locations=classpath:/mybatis/mapper/*.xml
```

**application.yml 示例：**

```yaml
mybatis-plus:
  mapper-locations: classpath:/mybatis/mapper/*.xml
```

在这两个示例中，`mybatis/mapper/` 是放置 XML 文件的目录，`*.xml` 表示加载该目录下所有的 XML 文件。

### 2. 在 Spring Boot 配置类中配置

你也可以在 Spring Boot 的配置类中通过 Java 代码来配置 XML 文件的位置。

```java
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean;
import org.apache.ibatis.session.SqlSessionFactory;

import javax.sql.DataSource;

@Configuration
@MapperScan("com.example.project.mapper")
public class MybatisPlusConfig {

    @Bean
    public SqlSessionFactory sqlSessionFactory(DataSource dataSource) throws Exception {
        MybatisSqlSessionFactoryBean sqlSessionFactory = new MybatisSqlSessionFactoryBean();
        sqlSessionFactory.setDataSource(dataSource);
        sqlSessionFactory.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath:/mybatis/mapper/*.xml"));
        return sqlSessionFactory.getObject();
    }
}
```

在这个配置类中，`@MapperScan` 注解指定了 mapper 接口的位置（即你的 mapper 接口所在的包），而 `sqlSessionFactory`
方法通过调用 `setMapperLocations` 方法来指定 XML 文件的位置。

gradle配置

如果您更改了 XML 文件的位置，并且这些位置不再遵循标准的 Gradle 资源目录结构（`src/main/resources`），您需要更新 Gradle
的配置以确保这些文件被正确识别为资源文件并包含在构建输出中。这里是如何做到这一点的一些步骤：

### 3. 更新 `sourceSets` 配置

您需要在 `build.gradle` 文件中更新 `sourceSets` 配置，以包含新的资源文件位置。假设您将 XML 文件移动到了 `src/main/mybatis`
目录下，您可以这样配置：

```groovy
sourceSets {
    main {
        java {
            srcDirs = ['src/main/java']
        }
        resources {
            // 添加新的资源目录
            srcDirs = ['src/main/resources', 'src/main/mybatis']
        }
    }
}
```

这段配置告诉 Gradle，除了标准的 `src/main/resources` 目录之外，`src/main/mybatis` 目录下的文件也应该被视为资源文件，并且包含在构建输出中。

### 4. 清理和重新构建项目

更改 `build.gradle` 文件后，您应该清理并重新构建项目，以确保更改生效。您可以使用以下命令：

```shell
./gradlew clean build
```

### 5. 检查 IDE 配置

如果您使用的是 IDE（如 IntelliJ IDEA），请确保 IDE 识别了您的新资源目录。有时，您可能需要手动标记这个目录为 “Resources
Root”。在 IntelliJ IDEA 中，您可以通过右键点击目录并选择 “Mark Directory as” > “Resources Root” 来实现。

### 6. 验证构建输出

构建项目后，您可以检查构建输出（通常位于 `build/resources/main` 目录下），以确保您的 XML 文件被正确复制到了那里。

通过以上步骤，您应该能够确保 Gradle 正确处理了新位置的 XML 文件。如果您仍然遇到问题，建议您检查构建日志，以便获取更多关于问题的信息。

### 7.使用processResources任务来进行配置

`processResources` 配置可以用于将特定的资源文件（在这个例子中是 XML 文件）从非标准的资源目录（如 `src/main/java`
）复制到构建输出中。这样的配置确保了在 Gradle 执行资源处理任务时，指定的 XML 文件会被正确地包含进去。

这里是您提供的配置的一个简单解释：

```groovy
processResources {
    from('src/main/java') {
        include '**/*.xml'
    }
}
```

- `processResources`：这是 Gradle 的一个任务，用于处理项目的资源文件。
- `from('src/main/java')`：指定了资源文件的来源目录。在这个例子中，它指向了 `src/main/java`，这通常用于存放 Java 源代码文件。
- `include '**/*.xml'`：这个指令告诉 Gradle 在来源目录及其所有子目录中包含所有扩展名为 `.xml` 的文件。

这种配置方式对于以下场景特别有用：

- 当您希望将位于 Java 源代码目录中的 XML 文件（如 MyBatis 的 Mapper XML 文件）作为资源文件处理。
- 当您不想改变文件的位置，但需要确保这些文件被包含在最终的构建输出中。

### 8.1注意事项1

  使用这种配置时，请确保它不会意外地包含不应作为资源文件处理的 XML 文件。因为 `include '**/*.xml'` 会匹配所有 XML
  文件，无论它们的用途。
  
  如果您的项目中有大量的资源文件分布在不同的目录中，您可能需要更复杂的配置来管理它们。在这种情况下，适当组织您的项目结构，并遵循标准的约定（如将资源文件放在 `src/main/resources`
  ）可能会更有益。

最后，确保在做出任何配置更改后重新构建您的项目，以验证更改是否按预期工作。

### 8.2注意事项2

- 确保你的 XML 文件位置与配置中指定的路径匹配。
- 如果你使用的是 Spring Boot，通常推荐使用 `application.properties` 或 `application.yml` 来配置，因为这样配置起来更为简单直观。
- 如果你需要更复杂的配置，例如使用不同的数据源，可能需要通过配置类来实现。

通过以上方法，你可以灵活地配置 Mybatis-Plus 中 XML 文件的位置，以满足项目的需求。

### 9.XML文件示例

根据 `Users` 实体类，我将为您提供一个示例 MyBatis XML 映射文件。这个文件将定义一些基本的
CRUD（创建、读取、更新、删除）操作。请注意，实际的 SQL 语句可能需要根据您的具体需求进行调整。

假设您的 XML 映射文件命名为 `UsersMapper.xml`，并且您打算将其放置在 `classpath:/mybatis/mapper/`
目录下（根据您的项目结构，这个位置可能有所不同）。

**UsersMapper.xml 示例：**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.myjob.mapper.UsersMapper">

    <!-- 定义结果映射 -->
    <resultMap id="UserResultMap" type="com.example.myjob.entity.Users">
        <id property="userId" column="user_id"/>
        <result property="username" column="username"/>
        <result property="loginName" column="login_name"/>
        <result property="password" column="password"/>
        <result property="email" column="email"/>
        <result property="role" column="role"/>
        <result property="registrationDate" column="registration_date"/>
        <!-- jwtToken 字段在数据库中不存在，因此不包含在映射中 -->
    </resultMap>

    <!-- 查询所有用户 -->
    <select id="selectAllUsers" resultMap="UserResultMap">
        SELECT user_id, username, login_name, password, email, role, registration_date
        FROM users
    </select>

    <!-- 根据 ID 查询用户 -->
    <select id="selectUserById" parameterType="int" resultMap="UserResultMap">
        SELECT user_id, username, login_name, password, email, role, registration_date
        FROM users
        WHERE user_id = #{userId}
    </select>

    <!-- 添加新用户 -->
    <insert id="insertUser" parameterType="com.example.myjob.entity.Users">
        INSERT INTO users (username, login_name, password, email, role, registration_date)
        VALUES (#{username}, #{loginName}, #{password}, #{email}, #{role}, #{registrationDate})
    </insert>

    <!-- 更新用户信息 -->
    <update id="updateUser" parameterType="com.example.myjob.entity.Users">
        UPDATE users
        SET username = #{username}, login_name = #{loginName}, password = #{password},
        email = #{email}, role = #{role}, registration_date = #{registrationDate}
        WHERE user_id = #{userId}
    </update>

    <!-- 根据 ID 删除用户 -->
    <delete id="deleteUserById" parameterType="int">
        DELETE FROM users
        WHERE user_id = #{userId}
    </delete>

</mapper>
```

请注意以下几点：

1. **Namespace**: `namespace` 属性应该与对应的 Mapper 接口的全限定名匹配。请根据您项目中的实际路径修改 `namespace` 的值。

2. **ResultMap**: 这里定义了一个结果映射（`UserResultMap`），用来映射 SQL 查询结果到 `Users` 实体类的属性。因为 `jwtToken`
   属性在数据库中不存在，所以它不包含在结果映射中。

3. **CRUD 操作**: 定义了一些基本的 CRUD 操作的 SQL 语句。您可能需要根据实际的数据库表结构和业务需求调整这些 SQL 语句。

4. **ParameterType**: 对于插入（`insert`）和更新（`update`）操作，`parameterType` 属性指定了参数的类型，它应该与您的实体类的全限定名匹配。

请确保您的实体类、Mapper 接口和 XML 文件之间的路径和命名是一致的，并且正确配置了 Mybatis-Plus 来加载这些 XML 文件。
