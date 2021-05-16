# SMBMS-超市订单管理系统

- JavaWeb项目-servlet实现
- ![image-20210305125602416](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/smbms-依赖.png)

- ![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210305125602416.png)

# 项目搭建准备工作

1. 搭建一个Maven Web项目

2. 配置Tomcat

3. 测试项目是否能运行

4. 导入项目中的jar包：

   jsp , servlet , mysql驱动 ， jstl , standard....

   ![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/smbms-架构.png)

5. 创建项目包结构

   ![image-20210305132741115](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210305150120442.png)

6. 编写实体类（com.joker.pojo）

   ORM映射：表-类映射

7. 编写基础公共类

   1. 数据库配置文件

      ```properties
      driver=com.mysql.jdbc.Driver
      url=jdbc:mysql://localhost:3306?useUnicode=true&characterEncoding=utf-8
      username=root
      password=123456
      ```

   2. 编写数据库的公共类

      ```java 
      //操作数据库 的公共类
      public class BaseDao {
          private static String driver;
          private static String url;
          private static String username;
          private static String password;
      
          //静态代码块，类加载的时候就初始化了
          static{
              Properties properties = new Properties();
              InputStream is = BaseDao.class.getClassLoader().getResourceAsStream("db.properties");
              try {
                  properties.load(is);
              } catch (IOException e) {
                  e.printStackTrace();
              }
      
              driver = properties.getProperty("driver");
              url = properties.getProperty("url");
              username = properties.getProperty("username");
              password = properties.getProperty("password");
          }
      //连接数据库
          public static Connection getConnection(){
              Connection connection = null;
              try {
                  Class.forName(driver);
                  connection = DriverManager.getConnection(url, username, password);
              } catch (Exception e) {
                  e.printStackTrace();
              }
      
              return connection;
          }
          //编写查询公共方法
          public static ResultSet excute(Connection connection,String sql,Object[] params,ResultSet resultSet,PreparedStatement preparedStatement) throws SQLException {
              preparedStatement = connection.prepareStatement(sql);
              for (int i = 0; i < params.length; i++) {
                  //setObject 占位符从1开始，，但是我们的数组从0开始
                  preparedStatement.setObject(i+1,params[i]);
              }
              //预编译的SQL，是直接执行，不用在这里传参
              resultSet = preparedStatement.executeQuery();
              return resultSet;
          }
          //编写增删改公共方法
          public static int excute(Connection connection,String sql,Object[] params,PreparedStatement preparedStatement) throws SQLException {
              preparedStatement = connection.prepareStatement(sql);
              for (int i = 0; i < params.length; i++) {
                  //setObject 占位符从1开始，，但是我们的数组从0开始
                  preparedStatement.setObject(i+1,params[i]);
              }
              int updateRows = preparedStatement.executeUpdate();
              return updateRows;
          }
          //关闭连接，释放资源
      public static boolean closeResource(Connection connection,PreparedStatement preparedStatement,ResultSet resultSet) {
          boolean flag = true;
      
          if (resultSet != null) {
              try {
                  resultSet.close();
                  //GC回收
                  resultSet = null;
              } catch (SQLException throwables) {
                  throwables.printStackTrace();
                  flag = false;
              }
          }
          if (preparedStatement != null) {
              try {
                  preparedStatement.close();
                  //GC回收
                  preparedStatement = null;
              } catch (SQLException throwables) {
                  throwables.printStackTrace();
                  flag = false;
              }
          }
          if (connection != null) {
              try {
                  connection.close();
                  //GC回收
                  connection = null;
              } catch (SQLException throwables) {
                  throwables.printStackTrace();
                  flag = false;
              }
          }
          return flag;
      }
      }
      ```

   3. 编写字符编码过滤器,并在web.xml中配置

      ```java
      public class CharacterEncodingFilter implements Filter {
      
          @Override
          public void init(FilterConfig filterConfig) throws ServletException {
      
          }
      
          @Override
          public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
              servletRequest.setCharacterEncoding("utf-8");
              servletResponse.setCharacterEncoding("utf-8");
              filterChain.doFilter(servletRequest,servletResponse);
          }
      
          @Override
          public void destroy() {
      
          }
      }
      ```

8. 导入静态资源（本地的资源）

## over

# 登录功能实现

![image-20210305150120442](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210305132741115.png)





1. 编写前端页面，login.jsp

2. web.xml里，将login.jsp设置为欢迎页面（首页）

   ```xml
       <welcome-file-list>
           <welcome-file>login.jsp</welcome-file>
       </welcome-file-list>
   ```

3. 表现Dao层登录用户登录的接口 UserDao

4. 编写Dao接口的实现类 UserDaoImpl

5. 业务层接口 UserService

6. 业务层接口的实现类 UserServiceImpl

7. 编写 servlet : LoginServlet 

8. 注册Servlet,:在web.xml中，

9. 测试访问，确保以上都能成功

## over

# 登录功能优化

## 注销

注销功能：

- 思路：移除Session，返回登录界面

- LogoutServlet
- web.xml里注册



## 用户拦截

- 未登录的用户不得进入
- 编写过滤器 SysFilter , 并注册

## over



# 密码修改

## 基本流程

1. 导入前端素材（在 common/header.jsp 里可以看到）pwdmodify.jsp
2. UserDao 接口添加方法：updatePwd()
3. UserDao 接口实现类 添加实现方法
4. UserService  接口添加方法updatePwd()
5. UserService  实现类实现方法
6. 编写UserSevlet ， 为了实现Servlet 的复用（多个用户），要提取出方法
7. web.xml里注册UserServlet
8. 测试

## 优化-AJAX验证旧密码

- 因为Ajax是异步请求，在输入，旧密码的时候，就可以验证了
- 观察js/pwdmodify.js 文件里的老密码的ajax

1. 阿里巴巴的fastjson依赖添加
2. 在UserServlet里提出方法 pwdModify()



# 用户管理实现

思路：

![image-20210306005220744](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210306005220744.png)

1. 导入分页的工具类 PageSupport
2. 用户列表页面的导入
   - userlist.jsp
   - rollpage.jsp 分页条页面

## 1. 获取用户数量

1. UserDao

   ```java
   public int getUserCount(Connection connection,String userName, int userRole) throws SQLException;
   ```

2. UserDaoImpl ,实现方法

3. UserService

   ```java
   //获取用户的数量,即记录数
   public int getUserCount(String userName,int userRole);
   ```

4. UserServiceImpl 实现方法

## 2. 获取用户列表

1. UserDao
2. UserDaoImpl
3. UserService
4. UserServiceImpl

## 3. 获取角色操作

- 为了职责分明，可以重新建一个包，职责分明，与pojo对应

1. RoleDao

   ```java
   public interface RoleDao {
       //获取角色列表
       public List<Role> getRoleList(Connection connection)throws Exception;
   
   }
   ```

2. RoleDaoImpl

3. RoleService

   ```java
   public interface RoleService {
       //获取角色列表
       public List<Role> getRoleList();
   }
   ```

4. RoleServiceImpl

##  4. 用户显示的Servlet

- 在UserServlet.java里添加 query() 方法

1. 获取用户前端的（查询）数据
2. 判断请求是否需要执行，看参数的值判断
3. 为了实现分页，需要计算出，当前页面，总页面，页面大小。。。
4. 用户列表展示
5. 返回前端

```java
 //重点，难点，查询用户列表
    public void query(HttpServletRequest req, HttpServletResponse resp){
        //从前端获取数据
        String queryUserName = req.getParameter("queryUserName");
        String temp = req.getParameter("queryUserRole");
        String pageIndex = req.getParameter("pageIndex");
        int queryUserRole=0;//默认给0 ，是因我如果什么都不给，就是空，

        //获取用户列表
        UserServiceImpl userService = new UserServiceImpl();
        List<User> userList = null;

        //第一次走这个请求，一定是第一页，且页面大小固定
        int pageSize = 5;//可以把这个配置到配置文件中，方便后期修改
        int currentPageNo=1;
        if(queryUserName == null){
            queryUserName = "";
        }
        if (temp != null && !temp.equals("")){
            queryUserRole = Integer.parseInt(temp);//给查询赋值，0.1.2.3
        }
        if (pageIndex != null){
            currentPageNo = Integer.parseInt(pageIndex);
        }
        //获取用户的总数
        int totalCount = userService.getUserCount(queryUserName, queryUserRole);
        //总页数支持
        PageSupport pageSupport = new PageSupport();
        pageSupport.setCurrentPageNo(currentPageNo);
        pageSupport.setPageSize(pageSize);
        pageSupport.setTotalCount(totalCount);
        //控制首页和尾页
        int totalPageCount = pageSupport.getTotalPageCount();
        //如果页面要小于1 了，就显示第一页
        if (currentPageNo < 1){
            currentPageNo = 1;
        }else if (currentPageNo >totalPageCount){//当前页大于了最后一页，
            currentPageNo = totalPageCount;
        }

        //获取用户列表展示
        userList = userService.getUserList(queryUserName, queryUserRole, currentPageNo, pageSize);
        req.setAttribute("userList",userList);
        //获取角色列表展示
        RoleServiceImpl roleService = new RoleServiceImpl();
        List<Role> roleList = roleService.getRoleList();
        req.setAttribute("roleList",roleList);
        req.setAttribute("totalCount",totalCount);
        req.setAttribute("totalPageCount",totalPageCount);
        req.setAttribute("currentPageNo",currentPageNo);
        req.setAttribute("queryUserName",queryUserName);
        req.setAttribute("queryUserRole",queryUserRole);


        //返回前端
        try {
            req.getRequestDispatcher("userlist.jsp").forward(req,resp);
        } catch (ServletException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }


    }
```



