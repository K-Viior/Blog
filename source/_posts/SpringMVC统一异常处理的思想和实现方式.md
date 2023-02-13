使用SpringMVC后，代码的调用者是SpringMVC框架，也就是说最终的异常会抛到SpringMVC中。最后由框架指定的异常处理类进行处理。
#### 1、创建一个自定义的异常处理类
实现HandlerExceptionResolver接口，并实现里面的异常处理方法，然后将这个类交给Spring管理。
#### 2、@ControllerAdvice注解
表示这是一个全局异常处理类。
在方法上加@ExceptionHandler注解，其中的value属性可以指定可以处理的异常类型。
