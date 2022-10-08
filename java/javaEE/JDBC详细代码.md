### PreparedStatement：
```java
import java.sql.*;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

/**
 * 怎么解决收起来注入
 * sql注入的根本原因是：先进行了字符串的拼接然后再进行编译
 *
 * java.sql.Statement接口的特点：先进行字符串拼接，然后再进行sql语句的编译。
 *      优点：使用Statement可以进行sql语句的拼接
 *      缺点：因为拼接的存在，导致可能给不法分子机会。导致sql注入
 *
 * java.sql.PreparedStatement接口的特点：先进行sql语句的编译，然后再进行sql语句的传值
 *      优点：避免sql注入
 *      缺点：没有办法进行sql语句的拼接，只能给sql语句传值
 *
 *      PreparedStatement预编译的数据库操作对象
 */
public class JDBCTest07 {
    public static void main(String[] args) {
        //登录
        Map<String,String> userLogin = initUI();

        //连接数据库验证用户名和密码是否正确
        boolean ok = checkNameAndPwd(userLogin.get("loginName"),userLogin.get("loginPwd"));
        System.out.println(ok ? "登录成功":"登录失败");

    }

    /**
     *验证用户名和密码
     * @param loginName 用户名
     * @param loginPwd  密码
     * @return 返回一个boolean类型，true表示成功，false表示失败。
     */
    private static boolean checkNameAndPwd(String loginName, String loginPwd) {
        Connection conn = null;
        PreparedStatement stmt = null;
        ResultSet rs = null;
        try {
            //注册驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
            //获取连接
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/helloworld","root","159357");
            //获取预编译的数据库操作对象
            //注意一个?是一个占位符，一个占位符只能接收一个值，或者说一个数据
            String sql = "select * from t_user where login_name = ? and login_pwd = ?";
            stmt = conn.prepareStatement(sql);//此时会发送sql语句给dbns，进行sql语句的编译
            //给占位符传值
            //JDBC中所有下标都是从1开始
            //怎么解决sql注入的，即使用户信息中有sql关键字，但是不参加编译就没事
            stmt.setString(1,loginName);//1代表第一个占位符”？“
            stmt.setString(2,loginPwd);
            //执行sql语句
            rs = stmt.executeQuery();//这个方法不需要将sql语句传递进去。
            //处理查询结果集
            if (rs.next()){
                return true;
            }
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        }finally {
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        return false;

    }

    private static Map<String,String> initUI() {
        //初始化一个信息，可以让用户输入用户名和密码
        System.out.println("欢迎使用该系统，请输入用户名和密码进行身份认证");
        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入用户名");
        String login_name = scanner.next();
        System.out.println("请输入密码");
        String login_pwd = scanner.next();
        //将用户名和密码放到map集合中，最后返回
        Map<String,String> userLogin = new HashMap<>();
        userLogin.put("loginName",login_name);
        userLogin.put("loginPwd",login_pwd);
        return userLogin;
    }
}
``````



### 事务：
```java
	import java.sql.*;
	public class JDBCTest10 {
	    public static void main(String[] args){
	        Connection conn = null;
	        PreparedStatement ps = null;
	        ResultSet rs = null;
	        try {
	            //注册驱动
	            Class.forName("com.mysql.cj.jdbc.Driver");
	            //获取连接
	            conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/helloworld", "root", "159357");
	            //开启事务，将自动提交自己关闭
	            conn.setAutoCommit(false);
	            //获取预编译操作对象
	            String sql = "update t_act set balance = ? where actno = ?";
	            ps = conn.prepareStatement(sql);
	            //给？传值
	            ps.setDouble(1, 10000);
	            ps.setString(2,"A");
	            int count = ps.executeUpdate();
	            ps.setDouble(1, 10000);
	            ps.setString(2,"B");
	            count += ps.executeUpdate();
	            System.out.println(count == 2 ? "转账成功" : "转账失败");
	
	            //代码能够执行到此处，说明代码没有任何异常，表示都成功了，手动提交
	            conn.commit();
	        } catch (ClassNotFoundException | SQLException e) {
	            //出现异常的话，为了保险起见。这里要回滚
	            try {
	                if (conn != null) {
			            //出现异常直接回滚
	                    conn.rollback();
	                }
	            } catch (SQLException ex) {
	                ex.printStackTrace();
	            }
	            e.printStackTrace();
	        } finally {
	            if (ps != null) {
	                try {
	                    ps.close();
	                } catch (SQLException e) {
	                    e.printStackTrace();
	                }
	            }
	            if (conn != null) {
	                try {
	                    conn.close();
	                } catch (SQLException e) {
	                    e.printStackTrace();
	                }
	            }
	        }
	    }
	}
```


### JDBC工具类：
````java
	public class DBUtil {
	    //工具类中的构造方法一般都是私有化的
	    //构造方法私有化是为了防止new对象，因为工具类的方法都是静态的，不需要new对象，直接用类名.的方法调用
	    //注意：Properties文件中的值尽量别带引号，带引号会默认将引号加载进去，可能导致文件读取错误
	    private DBUtil() {
	    }
	
	    //类加载的时候绑定属性资源文件
	    private static ResourceBundle bd = ResourceBundle.getBundle("resources/dp");
	
	    //注册驱动
	    static {
	        try {
	            Class.forName(bd.getString("driver"));
	        } catch (ClassNotFoundException e) {
	            e.printStackTrace();
	        }
	    }
	
	    /**
	     * 获取数据库连接对象
	     *
	     * @return 新的连接对象
	     * @throws SQLException 在主方法的位置进行了异常处理，这里直接上抛。
	     */
	    public static Connection getConnection() throws SQLException {
	        String url = bd.getString("url");
	        String user = bd.getString("user");
	        String password = bd.getString("password");
	        Connection conn = DriverManager.getConnection(url, user, password);
	        return conn;
	    }
	
	    /**
	     * 释放资源
	     *
	     * @param conn 连接对象
	     * @param stmt 数据库操作对象
	     * @param rs   查询结果集
	     */
	    public static void close(Connection conn, Statement stmt, ResultSet rs) {
	        if (rs != null) {
	            try {
	                rs.close();
	            } catch (SQLException e) {
	                e.printStackTrace();
	            }
	        }
	        if (stmt != null) {
	            try {
	                stmt.close();
	            } catch (SQLException e) {
	                e.printStackTrace();
	            }
	        }
	        if (conn != null) {
	            try {
	                conn.close();
	            } catch (SQLException e) {
	                e.printStackTrace();
	            }
	        }
	    }
	}
``````

### properties工具类
```properties
#mysql connectivity configuration###############

driver = com.mysql.cj.jdbc.Driver

url = jdbc:mysql://localhost:3306/helloworld

user = root

password = 159357
```