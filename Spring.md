# SpringBoot在调用run方法时发生了什么？

# SpringBoot的启动注解里包含了什么注解？

# SpringBoot相比Spring有什么优势？

# SpringMVC?

# DispatherServlet是做什么的？

# AOP的原理是什么？
## JDK和GCLib实现的动态代理的区别？

# 拦截器(Interceptor)和过滤器(Filter)有什么区别？
- 作用方式不同
    - 拦截器是横向拦截，对同等级上的接口进行前后置处理。
    - 过滤器可以理解成是一种责任链设计模式，对请求处理进行顺序传递，更像一种纵向拦截。
- 作用域不同
    - 过滤器依赖于servlet容器，只能在sevlet容器，web环境下使用。
    - 拦截器依赖于Spring容器，可以在Spring容器中调用，不管Spring此时处于什么环境。
- 中断链执行不同
  - 拦截器可以通过preHandle方法返回false进行中断。
  - 过滤器需要处理请求和响应对象来引发中断，需要额外操作，比如将请求重定向到错误页。
