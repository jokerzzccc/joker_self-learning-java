# Essays_07_ springBoot 全局异常统一处理



时间：2022年5月8日

参考博客：

- https://blog.csdn.net/Abysscarry/article/details/80400140 	
- https://blog.csdn.net/CSDN2497242041/article/details/121564454



简介：

- @ExceptionHandler：统一处理某一类异常，从而能够减少代码重复率和复杂度
- @ControllerAdvice：异常集中处理，更好的使业务逻辑与异常处理剥离开
- @ResponseStatus：可以将某种异常映射为HTTP状态码

# @ControllerAdvice

@ControllerAdvice 是Spring 3.2提供的新注解，可以对Controller中使用到@RequestMapping注解的方法做逻辑处理。

@ControllerAdvice ，很多初学者可能都没有听说过这个注解，实际上这是一个非常有用的注解。顾名思义，这是一个增强的 Controller，一般配合@ExceptionHandler使用来处理全局异常。注意不能自己try和catch异常，否则就不会被全局异常处理捕获到。

使用这个注解 ，可以实现三个方面的功能：

全局异常处理

全局数据绑定

全局数据预处理

灵活使用这三个功能，可以帮助我们简化很多工作，需要注意的是，这是 SpringMVC 提供的功能，在 Spring Boot 中可以直接使用，这里只介绍全局异常处理，需要其他功能可以访问参考链接。



Springboot对于全局异常的处理做了不错的支持，它提供了两个可用的注解。

@ControllerAdvice：用来开启全局的异常捕获

@ExceptionHandler：说明捕获哪些异常，对哪些异常进行处理。





# @RestControllerAdvice

- @RestControllerAdvice与@ControllerAdvice的区别就和@RestController与@Controller的区别类似，@RestControllerAdvice注解包含了@ControllerAdvice注解和@ResponseBody注解。
- 当自定义类加@ControllerAdvice注解时，方法需要返回json数据时，每个方法还需要**添加@ResponseBody**注解：
- 当自定义类加@RestControllerAdvice注解时，方法**自动返回json数据**，每个方法无需再添加@ResponseBody注解：

# @ExceptionHandler

- @ExceptionHandler：统一处理某一类异常，从而能够减少代码重复率和复杂度
- 当一个Controller中有方法加了@ExceptionHandler之后，这个Controller其他方法中没有捕获的异常就会以参数的形式传入加了@ExceptionHandler注解的那个方法中。
- **注**：如果单使用@ExceptionHandler，只能在当前Controller中处理异常。但当配合@ControllerAdvice一起使用的时候，就可以摆脱那个限制，对所有controller层异常进行处理。

# 自定义异常处理类总结

@ControllerAdvice + @ExceptionHandler 结合：

- 自定义一个异常处理类，处理所有 controller 层的异常，
- 只要是由 @RequestMapping 的接口。都可以捕捉到。
- 直接在业务层，throw new 一个自定义的异常处理就行。
- 业务层不可以自己 try-catch，不然自定义的异常捕捉就捕捉不到异常 了。



## **自定义全局异常处理：**

```java
/**
 * 全局的的异常拦截器（拦截所有的控制器）（带有@RequestMapping注解的方法上都会拦截）
 *
 * @author yj
 * @date 2018年05月22日 下午3:19:56
 */
@ControllerAdvice
public class GlobalExceptionHandler {

    private Logger log = LoggerFactory.getLogger(this.getClass());

    /**
     * 拦截业务异常
     */
    @ExceptionHandler(BussinessException.class)   //此处为自定义业务异常类
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)   //返回一个指定的http response状态码
    @ResponseBody
    public ErrorTip notFount(BussinessException e) {
        LogManager.me().executeLog(LogTaskFactory.exceptionLog(ShiroKit.getUser().getId(), e));
        getRequest().setAttribute("tip", e.getMessage());
        log.error("业务异常:", e);
        return new ErrorTip(e.getCode(), e.getMessage());
    }

    /**
     * 用户未登录
     */
    @ExceptionHandler(AuthenticationException.class)   //此处为shiro未登录异常类
    @ResponseStatus(HttpStatus.UNAUTHORIZED)
    public String unAuth(AuthenticationException e) {
        log.error("用户未登陆：", e);
        return "/login.html";
    }

    /**
     * 账号被冻结
     */
    @ExceptionHandler(DisabledAccountException.class)   //此处为shiro账号冻结异常类
    @ResponseStatus(HttpStatus.UNAUTHORIZED)
    public String accountLocked(DisabledAccountException e, Model model) {
        String username = getRequest().getParameter("username");
        LogManager.me().executeLog(LogTaskFactory.loginLog(username, "账号被冻结", getIp()));
        model.addAttribute("tips", "账号被冻结");
        return "/login.html";
    }

    /**
     * 账号密码错误
     */
    @ExceptionHandler(CredentialsException.class)   //此处为shiro账号密码错误异常类
    @ResponseStatus(HttpStatus.UNAUTHORIZED)
    public String credentials(CredentialsException e, Model model) {
        String username = getRequest().getParameter("username");
        LogManager.me().executeLog(LogTaskFactory.loginLog(username, "账号密码错误", getIp()));
        model.addAttribute("tips", "账号密码错误");
        return "/login.html";
    }

    /**
     * 验证码错误
     */
    @ExceptionHandler(InvalidKaptchaException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    public String credentials(InvalidKaptchaException e, Model model) {
        String username = getRequest().getParameter("username");
        LogManager.me().executeLog(LogTaskFactory.loginLog(username, "验证码错误", getIp()));
        model.addAttribute("tips", "验证码错误");
        return "/login.html";
    }

    /**
     * 无权访问该资源
     */
    @ExceptionHandler(UndeclaredThrowableException.class)
    @ResponseStatus(HttpStatus.UNAUTHORIZED)
    @ResponseBody
    public ErrorTip credentials(UndeclaredThrowableException e) {
        getRequest().setAttribute("tip", "权限异常");
        log.error("权限异常!", e);
        return new ErrorTip(BizExceptionEnum.NO_PERMITION);
    }

    /**
     * 拦截未知的运行时异常
     */
    @ExceptionHandler(RuntimeException.class)
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    @ResponseBody
    public ErrorTip notFount(RuntimeException e) {
        LogManager.me().executeLog(LogTaskFactory.exceptionLog(ShiroKit.getUser().getId(), e));
        getRequest().setAttribute("tip", "服务器未知运行时异常");
        log.error("运行时异常:", e);
        return new ErrorTip(BizExceptionEnum.SERVER_ERROR);
    }

    /**
     * session失效的异常拦截
     */
    @ExceptionHandler(InvalidSessionException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    public String sessionTimeout(InvalidSessionException e, Model model, HttpServletRequest request, HttpServletResponse response) {
        model.addAttribute("tips", "session超时");
        assertAjax(request, response);
        return "/login.html";
    }

    /**
     * session异常
     */
    @ExceptionHandler(UnknownSessionException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    public String sessionTimeout(UnknownSessionException e, Model model, HttpServletRequest request, HttpServletResponse response) {
        model.addAttribute("tips", "session超时");
        assertAjax(request, response);
        return "/login.html";
    }

    private void assertAjax(HttpServletRequest request, HttpServletResponse response) {
        if (request.getHeader("x-requested-with") != null
                && request.getHeader("x-requested-with").equalsIgnoreCase("XMLHttpRequest")) {
            //如果是ajax请求响应头会有，x-requested-with
            response.setHeader("sessionstatus", "timeout");//在响应头设置session状态
        }
    }

}
```



## **业务层的使用示例**：

```java
/**
 * 根据主键获取用户实体
 * @param id
 * @return
 */
public User selectById(String id) 
{
    User user = userMapper.selectByPrimaryId(id);
    if (user == null) {
        throw new GlobalExceptionHandler();
    }
    return user;
}
--------------------------------------
返回信息如下：
{
    "code": 50001,
    "data": false,
    "message": "用户不存在"
}
前端即可根据返回信息对用户进行友好的提示。
```