 # Spring 

## DI Dependency Injection 依赖注入

### bean的作用域

* 单例模式：bean对象至创建一次，默认机制，并发会出现问题
* 原型模式：每次get都创建都由第一个对象克隆而来，而不是每次都new一个。比较消耗资源
* session:
* request:
* web-socket：
* 这三种对应的就是web开发中的不同生命周期

## AOP 面向切面编程

> 通过实现接口实现，底层通过实现InvocationHandler接口实现

* pointcut切入点：就是需要被切入的方法
* 动态代理的是接口，需要被代理的方法的类需要实现一个接口
   1. 先将切面类写好
   2. 在容器中注入
   3. 在被切面对象在容器中被创建时，spring会偷偷拿着它和切面对象做一个反向代理，从而你getBean得到的对象不再通过是通过启动时依赖注入得到的对象，而是一个代理了被切面对象的对象。妙啊，真假孙悟空啊！

> 自定义类实现AOP，底层通过cglib实现

1. 构建切面，before，after方法
2. 在IOC 容器中注入
3. 然后写了配置文件就能直接使用

优点：相比于第一种通过接口实现的方法，这种AOP的方式会简单很多

缺点： 通过这种方式我们只能做一些简单的切面，没办法获得方法的信息，很多关于log的记录都没办法实现

## Restful风格编程

在URL不改变的条件下，通过改变请求的方式来进行增删改查。

```java
@RequestMapping("add/a/b")
public String test1(@PathVariable int a, @PathVariable int b){
  //todo
}
//Restful风格通过这种方式取参数
```

* 查询GET:
  
```java
     @GetMapping("url")
     @RequstMapping(name= 'url',method={RequestMethod.GET} )
  ```
  
* 删除DELETE:

   ```java
   @DeleteMapping()
   //..
   ```

   

* 更新POST:

   ```java
   @PostMapping("url")
   //..
   ```

* 增加PUT:

   ```java
   @PutMapping()
   //..
   ```

### 优缺点：

* ==优点==：
   * 最大的优点就是安全
   * 简洁，风格简单
   * 高效

