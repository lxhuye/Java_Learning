```java
@Test
    public void test6() {
        Connection conn = null;
        PreparedStatement ps = null;
        try {
            //1.建立sql连结
            Properties pro = new Properties();
            InputStream resourceAsStream = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");
            //将所需数据读入
            pro.load(resourceAsStream);
            String url = pro.getProperty("url");
            String user = pro.getProperty("user");
            String password = pro.getProperty("password");
            String driverClass = pro.getProperty("driverClass");
            //建立驱动
            Class.forName(driverClass);
            //建立连结
            conn = DriverManager.getConnection(url,user,password);

            //测试连结是否成功
//        System.out.println(conn);
            //2.获取preparedStatement实例
            String sql = "INSERT INTO Woman(NAME, AGE, HOBBY) value (?,?,?)";

            //3.填充占位符
            ps = conn.prepareStatement(sql);
            ps.setString(1,"CAROL");
            ps.setInt(2,18);
            ps.setString(3,"cooking");
            //执行preparedStatement
            ps.execute();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //4.关闭资源
            try {
                if(conn != null)
                    conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            try {
                if(ps != null)
                    ps.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

    }
```

