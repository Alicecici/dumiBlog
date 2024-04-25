---
nav:
  title: 拦截器
  order: 1
  second:
    title: SpringBoot全局拦截器
    order: 1
---
# SpringBoot全局拦截器

## 什么是拦截器？

拦截器：拦截器是基于SpringMVC的一种机制，它可以在请求到达控制器之前或之后进行拦截和处理，比如验证用户身份，记录日志，添加响应头等。

优点：拦截器可以获取到请求的上下文信息，如请求路径，参数，方法等，可以根据不同的业务逻辑进行灵活的处理。拦截器也可以配置多个，并指定拦截的顺序和范围，实现模块化和复用性。
缺点：拦截器也不能获取到请求方法的具体参数和返回值，只能获取到方法名和类名，这限制了它的功能。
适用场景：拦截器适合用于对请求进行简单的身份验证和权限检查，或者对请求和响应进行一些通用的处理，如日志记录，跨域设置等。

> 参考链接：https://blog.csdn.net/TaloyerG/article/details/132531393

## 为什么需要拦截器？

拦截器（Interceptor）在Spring框架中起到非常重要的作用，主要原因如下：

1. **实现横切关注点**：拦截器可以在请求处理的不同阶段执行自定义的逻辑，如日志记录、权限验证、事务管理等。通过拦截器，可以将这些横切关注点从业务逻辑中分离出来，提高代码的模块化和可维护性。

2. **重用性**：拦截器可以被多个处理器（Handler）共享，可以在不同的处理器中重复使用，避免了代码重复。这样可以提高代码的复用性，减少开发成本。

3. **解耦**：拦截器可以将不同的功能逻辑分离开来，使得各个功能之间解耦，提高系统的灵活性和可扩展性。比如，可以将日志记录、异常处理、权限验证等功能独立出来，方便单独管理和修改。

4. **控制请求流程**：拦截器可以在请求处理的不同阶段介入，可以在请求处理前、处理后或异常处理时执行特定逻辑。这样可以灵活控制请求的处理流程，实现定制化的请求处理逻辑。

5. **性能优化**：拦截器可以对请求进行预处理、后处理，可以进行缓存、压缩、日志记录等操作，从而优化系统性能。拦截器可以在请求进入Controller层之前对请求进行处理，减轻Controller的负担。

总的来说，拦截器是Spring框架中非常重要的组件，可以实现横切关注点、重用性、解耦、控制请求流程和性能优化等功能，帮助开发者更好地管理和控制请求处理流程。

## Spring Boot中使用拦截器

下面是一个简单的示例，演示如何在Spring Boot 3中使用拦截器（Interceptor）进行请求头中的token鉴权：

1. 创建一个拦截器类`AuthInterceptor`，实现`HandlerInterceptor`接口：

```java
import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class AuthInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        String token = request.getHeader("Authorization");

        // 进行token验证逻辑，例如验证token是否有效
        if ("valid_token".equals(token)) {
            return true; // 验证通过，继续执行后续操作
        } else {
            response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
            return false; // 验证失败，拦截请求
        }
    }
}
```

2. 创建一个配置类`WebConfig`，注册拦截器并配置拦截路径：

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new AuthInterceptor())
                .addPathPatterns("/**"); // 拦截所有请求
    }
}
```

3. 在Spring Boot应用程序的启动类中添加注解`@SpringBootApplication`和`@ComponentScan`：

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ComponentScan;

@SpringBootApplication
@ComponentScan(basePackages = "com.example.interceptorexample")
public class InterceptorExampleApplication {

    public static void main(String[] args) {
        SpringApplication.run(InterceptorExampleApplication.class, args);
    }
}
```

在这个示例中，拦截器`AuthInterceptor`会拦截所有请求，并在请求头中查找名为"Authorization"的token进行验证。如果token为"
valid_token"，则请求会继续执行；否则返回401 Unauthorized状态码。你可以根据实际需求修改验证逻辑和拦截路径。

## 关于权限不足的提示

在上面的代码中，我们只是返回了一个http的状态码，这显然是信息不足的，在正常的系统中，权限不足分为很多种情况，比如：

1. 用户未登录
2. 用户登录但是没有权限
3. 用户登录但是token过期
4. 用户登录但是token无效
5. 用户登录但是token被篡改
6. 用户登录但是token被禁用

这些情况都需要有不同的提示，所以我们需要在返回的状态码中加入一些提示信息，比如：

```java
package com.example.myjob.config;

import com.alibaba.fastjson2.JSONObject;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.http.HttpHeaders;
import org.springframework.web.servlet.HandlerInterceptor;

import java.nio.charset.StandardCharsets;

public class AuthInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        String token = request.getHeader(HttpHeaders.AUTHORIZATION);

        // 进行token验证逻辑，例如验证token是否有效
        if ("valid_token".equals(token)) {
            return true; // 验证通过，继续执行后续操作
        } else {
            // 设置编码为UTF-8
            response.setCharacterEncoding(StandardCharsets.UTF_8.name());
            response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
            JSONObject jsonObject = new JSONObject();
            jsonObject.put("message", "token验证失败");
            jsonObject.put("code", 401);
            jsonObject.put("data", null);
            response.setContentType("application/json");
            response.getWriter().write(jsonObject.toJSONString());
            return false; // 验证失败，拦截请求
        }
    }
}
```

> 注意：确保在写入JSON数据之前设置编码。否则会出现乱码问题。

## 在拦截器中进行数据库查询

在实际的业务场景中，我们可能需要在拦截器中进行数据库查询，以验证用户的token是否有效

在拦截器中，你可以注入上面创建的Mapper接口，并在preHandle、postHandle或afterCompletion方法中使用它来执行数据库操作。

```java
@Component
public class MyInterceptor implements HandlerInterceptor {

    @Autowired
    private UserMapper userMapper;

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // 例如，在请求处理之前验证用户
        Long userId = ... // 从请求中获取用户ID
        User user = userMapper.findUserById(userId);
        if (user == null) {
            response.setStatus(HttpStatus.UNAUTHORIZED.value());
            return false;
        }
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        // 请求处理之后的逻辑
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        // 请求完成之后的逻辑
    }
}
```

需要在Spring Boot配置中注册这个拦截器，以便它可以拦截请求。

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Autowired
    private MyInterceptor myInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(myInterceptor);
    }
}
```

注意事项

- 确保你的Mapper接口被Spring管理，通常通过在接口上添加@Mapper注解或在配置类上使用@MapperScan注解。
- 在拦截器中注入Mapper接口，确保拦截器也被Spring管理。可以通过在拦截器类上添加@Component注解来实现。
  -

可以在preHandle、postHandle或afterCompletion方法中执行数据库操作。preHandle适用于在请求处理之前执行操作，postHandle适用于在请求处理之后但在视图渲染之前执行操作，afterCompletion适用于请求完成后执行操作，无论请求处理成功还是失败。

### @Autowired private MyInterceptor myInterceptor; 和我直接new MyInterceptor的区别在哪?

在Spring框架中使用`@Autowired`注解与直接使用`new`
关键字来创建对象之间有几个重要的区别。这些区别主要涉及到Spring的依赖注入（DI）机制、对象的生命周期管理以及Spring提供的其他特性。下面详细解释这些区别：

### 1. 依赖注入和控制反转

- **使用`@Autowired`：** 当你使用`@Autowired`
  注解时，你是在告诉Spring框架，你希望它在运行时自动将匹配的bean注入到该字段中。这是依赖注入（DI）的一种形式，也是控制反转（IoC）的一个实例，因为你将对象的创建和依赖管理的控制权交给了Spring容器。

- **直接使用`new`关键字：** 当你直接使用`new`
  关键字创建对象时，你是在显式地控制对象的创建过程。这意味着Spring框架不会自动管理这个对象的生命周期，也不会注入任何依赖项。这个对象就是一个普通的Java对象，并不是Spring管理的bean。

### 2. Spring的特性支持

- **使用`@Autowired`：** 当你让Spring通过`@Autowired`
  来注入依赖时，被注入的对象可以自动享受Spring提供的各种特性，如依赖注入、生命周期管理、事务管理、AOP代理等。如果你的拦截器（或其他对象）需要访问数据库或需要其他Spring管理的bean，使用`@Autowired`
  是必要的。

- **直接使用`new`关键字：** 使用`new`
  关键字创建的对象不会自动获得Spring框架提供的任何特性。这意味着，如果你的对象需要依赖其他Spring管理的bean，或者你希望利用Spring提供的特性，直接实例化就不是一个好选择。

### 3. 对象的生命周期

- **使用`@Autowired`：** Spring容器负责管理通过`@Autowired`注入的对象的生命周期。这包括创建对象、注入依赖、管理对象的作用域（如单例、原型等）以及在适当的时候销毁对象。

- **直接使用`new`关键字：** 使用`new`关键字创建的对象的生命周期完全由你自己控制。Spring框架不会介入这些对象的生命周期管理。

### 结论

总的来说，如果你在Spring应用中工作，推荐使用`@Autowired`
（或其他依赖注入的方式）来获取Spring管理的bean，以便充分利用Spring框架提供的各种特性和优势。直接使用`new`
关键字创建对象在某些特定情况下可能有用，但这通常意味着你不能利用Spring提供的依赖注入和生命周期管理等特性。对于需要与Spring框架集成的组件（如拦截器、控制器、服务等），使用Spring的依赖注入机制是最佳实践。

## 遇到的问题

在SpringBoot项目中，当你使用`InterceptorRegistry`的`addInterceptor`方法添加拦截器，并且同时在`WebMvcConfigurer`
的`addCorsMappings`方法中通过`CorsRegistry`的`addMapping`
方法配置跨域时，可能会出现配置的跨域失效的情况。这通常是因为拦截器中的预处理方法（`preHandle`
）在跨域的预检请求（OPTIONS请求）到达Controller之前执行，而这个预处理方法默认没有对OPTIONS请求进行放行，导致跨域的预检请求被拦截，从而使得跨域配置失效。

要解决这个问题，你需要在你的拦截器中显式地检查请求的类型，并对OPTIONS请求进行放行。这可以通过修改拦截器的`preHandle`
方法来实现。下面是一个简单的例子：

```java
import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class MyInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // 检查是否是OPTIONS请求
        if ("OPTIONS".equalsIgnoreCase(request.getMethod())) {
            // 是OPTIONS请求，直接放行
            return true;
        }
        // 非OPTIONS请求，继续其他处理
        // 这里可以添加你自己的逻辑
        return true;
    }
}
```

在上面的代码中，`preHandle`方法首先检查请求是否是OPTIONS请求，如果是，则直接返回`true`以放行该请求。这样，预检请求就不会被拦截，从而允许跨域配置正常工作。

请注意，这只是解决跨域配置失效问题的一种方法。具体情况可能因项目的具体配置和需求而有所不同。如果遇到其他相关问题，可能需要根据具体情况进行调整。
