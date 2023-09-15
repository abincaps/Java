
## JDBC

## JDBC

- Java DataBase Connectivity 

## 注册驱动的方法

- 只做一次注册驱动
- `DriverManager.registerDriver(new com.mysql.cj.jdbc.Driver());`
- `System.setProperty("jdbc.drivers", "com.mysql.cj.jdbc.Driver");` 
- `Class.forName("com.mysql.cj.jdbc.Driver");` 

## 建立连接

- `DriverManager.getConnection("jdbc:mysql://localhost:3306/", " user", "password");`
- `url 形式为 jdbc:subprotocol:subname//host:port属性=值&属性=值`

## Connection

- `Statement  createStatement()` 创建一个 `Statement` 对象用于将 SQL 语句发送到数据库
- `PreparedStatement  prepareStatement(String  sql)`  创建一个 `PreparedStatement` 对象用于将参数化 SQL 语句发送到数据库, `sql` 可能包含一个或多个“?”的 SQL 语句参数占位符, 解决sql注入问题

## Statement

- 使用多个#Statement分开查询和更新
- `ResultSet  executeQuery(String  sql)` 执行给定的静态 SQL 语句
- `int executeUpdate(String  sql)`  执行给定的静态 SQL 语句，`INSERT`, `UPDATE`, `DELETE` 语句或不返回任何内容的 SQL 语句, 返回受影响的行数

## ResultSet接口

- 表示数据库结果集的数据
- `boolean next()` `ResultSet` 游标最初位于第一行之前, 不是在第一行，第n次调用方法 `next` 使第n行成为当前行
- `Object  getObject(int columnIndex)` 获取当前行中指定列号的值
- `Object  getObject(String  columnLabel)` 获取当前行中指定列名的值

## PreparedStatement

- `void setInt(int parameterIndex, int x)` 将指定参数设置为给定的 `int` 值, 和第几个 `？` 占位符绑定

## 静态方法封装JDBC工具类

```java
public class JdbcUtils {  
    private static final String URL = "jdbc:mysql://192.168.80.1:3306/demo";  
    private static final String USER = "root";  
    private static final String PASSWORD = "";  
  
    // 阻止实例化  
    private JdbcUtils() {  
    }  
  
    // 注册驱动  
    static {  
        try {  
            Class.forName("com.mysql.cj.jdbc.Driver");  
        } catch (ClassNotFoundException e) {  
            throw new RuntimeException(e);  
        }  
    }  
  
    // 获取连接  
    public static Connection getConnection() throws SQLException {  
        return DriverManager.getConnection(URL, USER, PASSWORD);  
    }  
  
    // 释放资源  
    public static void free(ResultSet rs, Statement st, Connection con) {  
        try {  
            if (rs != null) {  
                rs.close();  
            }  
        } catch (SQLException e) {  
            e.printStackTrace();  
        } finally {  
  
            try {  
                if (st != null) {  
                    st.close();  
                }  
            } catch (SQLException e) {  
                e.printStackTrace();  
            } finally {  
                  
                try {  
                    if (con != null) {  
                        con.close();  
                    }  
                } catch (SQLException e) {  
                    e.printStackTrace();  
                }  
            }  
        }  
    }  
}
```

## 单例模式封装JDBC工具类

```java
public class JdbcUtils {  
    private static final String URL = "jdbc:mysql://192.168.80.1:3306/demo";  
    private static final String USER = "root";  
    private static final String PASSWORD = "";  
  
    private static JdbcUtils instance = null;  
  
    // 阻止实例化  
    private JdbcUtils() {  
  
    }  
  
    // 注册驱动  
    static {  
        try {  
            Class.forName("com.mysql.cj.jdbc.Driver");  
        } catch (ClassNotFoundException e) {  
            throw new RuntimeException(e);  
        }  
    }  
  
    // 获取对象， 懒汉单例模式  
    public static JdbcUtils getInstance() {  

        if (instance == null) {  
            instance = new JdbcUtils();  
        }  
  
        return instance;  
    }  
  
    public Connection getConnection() throws SQLException {  
        return DriverManager.getConnection(URL, USER, PASSWORD);  
    }  
}
```


## 数据库连接池

- 数据库连接池负责分配、管理和释放数据库连接

## Druid

- Druid 是一个 JDBC 组件库

```java
# druid.properties 文件
driveClassName=com.mysql.cj.jdbc.Driver  
url=jdbc:mysql://192.168.80.1:3306/demo  
username=root  
password=  
initialSize=5  
maxActive=10  
maxWait=1000
```

## 封装Druid工具类

```java
public class JdbcUtils {  
    private static DataSource ds = null;  
  
    // 加载配置  
    static {  
  
        Properties prop = new Properties();  
        InputStream is = JdbcUtils.class.getClassLoader().getResourceAsStream("druid.properties");  
  
        try {  
            prop.load(is);  
            ds = DruidDataSourceFactory.createDataSource(prop);  
        } catch (Exception e) {  
            throw new RuntimeException(e);  
        }  
    } 
    
	// 获取 DataSource
	public static DataSource getDataSource() {  
	    return ds;  
	}  
}
```

## Druid数据源配置

```java
try (DruidDataSource dataSource = new DruidDataSource()) {  

    dataSource.setDriverClassName(MYSQL_DRIVER);  
    dataSource.setUrl(URL);  
    dataSource.setUsername(USER);  
    dataSource.setPassword(PASSWORD);  
    DruidPooledConnection con = dataSource.getConnection(); 
      
} catch (SQLException e) {  
    e.printStackTrace();  
}
```
