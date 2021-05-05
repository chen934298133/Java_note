## 手写版本

<details>
<summary>&#127808; 注释版本 &#127808;</summary>
  
步骤：
1. 加载驱动
2. 用户信息和URL
3. 连接成功，数据库对象(getConnection)
4. 执行SQL的对象(connection.createStatement())
5. 执行SQL的对象去执行SQL，可能存在结果，查看返回结果(ResultSet resultSet = statement.executeQuery(sql))
6. 释放连接
```java
public class JDBC_Demo01 {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        // 1. 加载驱动
        Class.forName("com.mysql.jdbc.Driver");
        // 2. 用户信息和URL
        String url = "jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf8&useSSL=true";
        String useName = "root";
        String password = "701217";
        // 3. 连接成功，数据库对象
        // connection 即代表拿到的数据库
        Connection connection = DriverManager.getConnection(url, useName, password);
        // 4. 执行SQL的对象
        Statement statement = connection.createStatement();
        // 5. 执行SQL的对象去执行SQL，可能存在结果，查看返回结果
        String sql = null;
        int id = 10;
        sql = "SELECT * FROM test.sys_areatab a order by a.id asc limit 5";
        sql = "SELECT * FROM test.sys_areatab a where a.id < '"+id+"' ";
//        statement.executeQuery()      查找
//        statement.executeUpdate()     更新(插入、删除)
        ResultSet resultSet = statement.executeQuery(sql);      // 返回的结果集，封装了我们全部查询出来的结果

        int i = 1;
        while (resultSet.next()){
            System.out.print(i++ + ". ");
            System.out.print("id=" + resultSet.getObject("id")+ " ");
            System.out.print("code=" + resultSet.getObject("code") + " ");
            System.out.print("name=" + resultSet.getObject("name")+ " ");
            System.out.println("parentCode=" + resultSet.getObject("parentCode"));
        }
        // 6. 释放连接
        resultSet.close();
        statement.close();
        connection.close();
    }
}

```
</details>
<details>
<summary>&#127808; 调用jdbc的例子 &#127808;</summary>


```java
    /**
     * 计算应该抽取的条数
     */
    public int getNum() {

        int num = 0;
        // JDBC 数据准备
        String sql = null;
        ResultSet resultSet = null;
        Statement statement = null;
        Connection connection = null;

        try {
            // 连接数据库对象
            connection = ConnectionDB.connDB();
            // 执行SQL的对象
            statement = connection.createStatement();

            sql = "select FLOOR(COUNT( DISTINCT f.FWJLFWBH) * " + extractParameter + "  ) extractNum\n" +
                    "from platform.fwjl f, platform.lrxx l, yl_fwnr n, sys_dept d\n" +
                    "where f.FWJLLRBH = l.lrxxcode\n" +
                    "and f.FWJLFWNR = n.fwnrid\n" +
                    "and d.org_code = l.lrxxssjg\n" +
                    "and f.TYPE = '0'\n" +
                    "and f.FWJLKSSJ > CURDATE() \n" +
                    "and f.FWJLJSSJ is not null\n" +
                    "and f.FWJLGDZT = '0'\n" +
                    "and (f.state = '1' or f.state is null)";

            PreparedStatement pst = connection.prepareStatement(sql);

            boolean execute = pst.execute();
//            execute = statement.execute(sql);
            if (execute) {
//            resultSet = statement.executeQuery(sql);
                // 获取返回值
                resultSet = pst.executeQuery();
                if (resultSet != null) {
                    // 遍历返回的Set集合
                    while (resultSet.next()) {
                        num = resultSet.getInt("extractNum");
                    }

                } else {
                    num = -1;
                }
            } else {
                num = -1;
            }

        } catch (Exception e) {
            System.out.println("JDBC插入失败");
            e.printStackTrace();
        } finally {
//            System.out.println("JDBC插入成功");
            // 释放资源
            if (connection != null) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (statement != null) {
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (resultSet != null) {
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }

        return num;
    }
```
</details>

<details>
<summary>&#127808; 封装的ConnectionDB类 &#127808;</summary>

经常使用获取connection，便于统一一下使用的数据库
```java
    /**
     * 计算应该抽取的条数
     */
    public int getNum() {

        int num = 0;
        // JDBC 数据准备
        String sql = null;
        ResultSet resultSet = null;
        Statement statement = null;
        Connection connection = null;

        try {
            // 连接数据库对象
            connection = ConnectionDB.connDB();
            // 执行SQL的对象
            statement = connection.createStatement();

            sql = "select FLOOR(COUNT( DISTINCT f.FWJLFWBH) * " + extractParameter + "  ) extractNum\n" +
                    "from platform.fwjl f, platform.lrxx l, yl_fwnr n, sys_dept d\n" +
                    "where f.FWJLLRBH = l.lrxxcode\n" +
                    "and f.FWJLFWNR = n.fwnrid\n" +
                    "and d.org_code = l.lrxxssjg\n" +
                    "and f.TYPE = '0'\n" +
                    "and f.FWJLKSSJ > CURDATE() \n" +
                    "and f.FWJLJSSJ is not null\n" +
                    "and f.FWJLGDZT = '0'\n" +
                    "and (f.state = '1' or f.state is null)";

            PreparedStatement pst = connection.prepareStatement(sql);

            boolean execute = pst.execute();
//            execute = statement.execute(sql);
            if (execute) {
//            resultSet = statement.executeQuery(sql);
                // 获取返回值
                resultSet = pst.executeQuery();
                if (resultSet != null) {
                    // 遍历返回的Set集合
                    while (resultSet.next()) {
                        num = resultSet.getInt("extractNum");
                    }

                } else {
                    num = -1;
                }
            } else {
                num = -1;
            }

        } catch (Exception e) {
            System.out.println("JDBC插入失败");
            e.printStackTrace();
        } finally {
//            System.out.println("JDBC插入成功");
            // 释放资源
            if (connection != null) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (statement != null) {
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (resultSet != null) {
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }

        return num;
    }
```
</details>

## spring整合版本

<details>
<summary>&#127808; 调用jdbc的例子 &#127808;</summary>


```java
    @PostMapping(value = "/objectTypeList1")
    @ResponseBody
    public List<Map<String, Object>> objectTypeList(String code) {

        String sql = "select count( distinct CASE WHEN LRXXNEW = 1 THEN LRXXCODE END ) as lrxx_ju, \n" +
                "COUNT(distinct CASE WHEN LRXXNEW = 1 AND LRXXSEXX = 1 THEN LRXXCODE END ) AS ju_male,\n" +
                "COUNT(distinct CASE WHEN LRXXNEW = 1 AND LRXXSEXX = 2 THEN LRXXCODE END ) AS ju_female,\n" +
                "count(distinct CASE WHEN LRXXNEW = 2 THEN LRXXCODE END ) as lrxx_zheng,\n" +
                "COUNT(distinct CASE WHEN LRXXNEW = 2 AND LRXXSEXX = 1 THEN LRXXCODE END ) AS zheng_male,\n" +
                "COUNT(distinct CASE WHEN LRXXNEW = 2 AND LRXXSEXX = 2 THEN LRXXCODE END ) AS zheng_female\n" +
                " FROM platform.lrxx l, sys_dept d\n" +
                " WHERE l.LRXXSSJG = d.org_code\n" +
                "  and LRXXSTATUS = 1 and d.street = '10800102'";
  // 获取方法一： 得到List结果集
        List<Map<String, Object>> list = jdbcTemplate.queryForList(sql);
  // 获取方法二： 得到Map单一结果
        Map<String, Object> list = jdbcTemplate.queryForMap(sql);
  // 获取方法三: 得到set结果集
        SqlRowSet sqlRowSet = jdbcTemplate.queryForRowSet(sql, new Object[]{code, "2021-04-23"});
        while (sqlRowSet.next()) {
            System.out.println(sqlRowSet.getString("id"));
        }
        return list;
    }
```
</details>


## 预编译 手写版本

<details>
<summary>&#127808; 预编译手写版本 &#127808;</summary>


```java
        /**
     * 备用方法
     */
    @ResponseBody
    @PostMapping("/listJDBC")
    public BaseResult ListJDBC(FwlrVOByEighty newEightyElder) {

        newEightyElder.setArea(ShiroUtils.getSysUser().getDept().getArea());
        if (ShiroUtils.getSysUser().getDept().getDeptType() == 8) {
            newEightyElder.setStreet(ShiroUtils.getSysUser().getDept().getStreet());
        }

        List<FwlrVOByEighty> resultList = new ArrayList<>();

        String sql = null;
        ResultSet resultSet = null;
        Statement statement = null;
        Connection connection = null;
        List<Object> list = new ArrayList<>();

        try {
            // 连接数据库对象
            connection = getConnection();
            // 执行SQL的对象
            statement = connection.createStatement();

            sql = "SELECT \n" +
                    "    (select a.`name` from yl_dev_test.sys_areatab a where o.area = a.`code` limit 1) area,\n" +
                    "    (select b.`name` from yl_dev_test.sys_areatab b where o.street = b.`code` limit 1) street,\n" +
                    "    (select c.`name` from yl_dev_test.sys_areatab c where o.block = c.`code` limit 1) comm,\n" +
                    "    fwjllrxm,\n" +
                    "    fwjlorg,\n" +
                    "    group_concat(DISTINCT fwnr) fwnr,\n" +
                    "    count(distinct f.id ) AS total,\n" +
                    "    sum(TRUNCATE((UNIX_TIMESTAMP(fwjljssj) - UNIX_TIMESTAMP(fwjlkssj)) / 3600,2)) AS totalDuration,\n" +
                    "    l.code as lrxxcode,\n" +
                    "    gov.nursing_level LRXXHLDJ,\n" +
                    "    CASE\n" +
                    "    WHEN LENGTH( l.code )= 18 THEN\n" +
                    "    YEAR (\n" +
                    "    now()) - YEAR (\n" +
                    "    substring( l.code, 7, 8 ))\n" +
                    "    WHEN LENGTH( l.code )= 15 THEN\n" +
                    "    YEAR (\n" +
                    "    now()) - YEAR (\n" +
                    "    STR_TO_DATE( CONCAT( '19', substring(l.code, 7, 2 )), '%Y' )) ELSE NULL\n" +
                    "    END as age\n" +
                    "    FROM\n" +
                    "    platform.fwjl f\n" +
                    "    left join sys_dept o on f.fwjldeptid = o.org_code\n" +
                    "    left join lrxx_base l on l.CODE = f.FWJLLRBH\n" +
                    "    left join lrxx_base_government_purchase gov on l.CODE = gov.lrxx_code\n" +
                    "    left join lrxx_base_type lt on lt.lrxx_id = l.id" +
                    "    where f.type = 0 and f.FWJLFWZT = 2 and (f.AppVersion is null or f.AppVersion not like '%导入%' ) and lt.lrxx_type = 2 ";

            if (newEightyElder.getOrgName() != null && newEightyElder.getOrgName() != "") {
                sql += " and f.fwjlorg like ? ";
            }
            if (newEightyElder.getFwjllrlb() != null && newEightyElder.getFwjllrlb() != "") {
                sql += " and f.fwjllrlb = ? ";
            }
            if (newEightyElder.getFwjlkssj() != null) {
                sql += " and f.fwjlkssj >= ? ";
            }
            if (newEightyElder.getFwjljssj() != null) {
                sql += " and f.fwjlkssj <= ? ";
            }
            if (newEightyElder.getArea() != null && newEightyElder.getArea() != "") {
                sql += " and o.area = ? ";
            }
            if (newEightyElder.getStreet() != null && newEightyElder.getStreet() != "") {
                sql += " and o.street like ? ";
            }
            if (newEightyElder.getComm() != null && newEightyElder.getComm() != "") {
                sql += " and o.block like ? ";
            }
            if (newEightyElder.getLRXXHLDJ() != null && newEightyElder.getLRXXHLDJ() != "") {
                sql += " and gov.nursing_level = ? ";
            }

            sql += " group by fwjllrbh order by f.fwjlkssj desc limit ?,? ";

            PreparedStatement pst = connection.prepareStatement(sql);

//            // 按服身份idCard查询
//            pst.setObject(1, idCard);
//            // 开始时间过滤
//            pst.setDate(2, new java.sql.Date(fwlrVOByNew.getFwjlkssj().getTime()));
//            // 结束时间过滤
//            pst.setDate(3, new java.sql.Date(fwlrVOByNew.getFwjljssj().getTime()));

            int i = 1;
            // 机构名称
            if (newEightyElder.getOrgName() != null && newEightyElder.getOrgName() != "") {
                pst.setString(i, "%" + newEightyElder.getOrgName() + "%");
                i++;
            }
            // 人员类别
            if (newEightyElder.getFwjllrlb() != null && newEightyElder.getFwjllrlb() != "") {
                pst.setString(i, newEightyElder.getFwjllrlb());
                i++;
            }
            // 开始时间
            if (newEightyElder.getFwjlkssj() != null) {
                pst.setDate(i, new java.sql.Date(newEightyElder.getFwjlkssj().getTime()));
                newEightyElder.setDate(new java.sql.Date(newEightyElder.getFwjlkssj().getTime()));
                i++;
            }
            // 结束时间
            if (newEightyElder.getFwjljssj() != null) {
//                pst.setDate(i, new java.sql.Date(newEightyElder.getFwjljssj().getTime() + 24 * 60 * 60 * 1000));
                pst.setDate(i, new java.sql.Date(newEightyElder.getFwjljssj().getTime()));
                i++;
            }
            // 区
            if (newEightyElder.getArea() != null && newEightyElder.getArea() != "") {
                pst.setString(i, newEightyElder.getArea());
                i++;
            }
            // 街道
            if (newEightyElder.getStreet() != null && newEightyElder.getStreet() != "") {
                pst.setString(i, newEightyElder.getStreet() + "%");
                i++;
            }
            // 社区
            if (newEightyElder.getComm() != null && newEightyElder.getComm() != "") {
                pst.setString(i, newEightyElder.getComm() + "%");
                i++;
            }
            // 护理等级
            if (newEightyElder.getLRXXHLDJ() != null && newEightyElder.getLRXXHLDJ() != "") {
                pst.setString(i, newEightyElder.getLRXXHLDJ());
                i++;
            }
            // limit 分页
            pst.setInt(i, newEightyElder.getPageSize() * (newEightyElder.getPageNum() - 1));
            i++;
            pst.setInt(i, newEightyElder.getPageSize());

            // 执行
            boolean execute = pst.execute();
            if (execute) {
                // 获取返回值
                resultSet = pst.executeQuery();
                if (resultSet != null) {

                    // 遍历返回的Set集合
                    while (resultSet.next()) {
                        FwlrVOByEighty obj = new FwlrVOByEighty();
                        obj.setArea(resultSet.getString("area"));
                        obj.setStreet(resultSet.getString("street"));
                        obj.setComm(resultSet.getString("comm"));
                        obj.setFwjllrxm(resultSet.getString("fwjllrxm"));
                        obj.setLRXXHLDJ(resultSet.getString("LRXXHLDJ"));
                        obj.setTotal(resultSet.getInt("total"));
                        obj.setTotalDuration(resultSet.getDouble("totalDuration"));
                        obj.setAge(resultSet.getString("age"));
                        obj.setIdCard(resultSet.getString("lrxxcode"));
                        obj.setOrgName(resultSet.getString("fwjlorg"));
//                        obj.setNumber(2);
                        resultList.add(obj);

                        System.out.println(resultSet.getString("lrxxcode"));

//                        obj.setAssessInfo(dayT + "/" + dayF);
//                        System.out.println(dayT + "/" + dayF);
                        // 每月达标考核
                        if (obj.getTotal() > 2) {
                            obj.setMonthQualified(true);
                        } else {
                            obj.setMonthQualified(false);
                        }

                        // 每日信息: serviceRecordByDay
                        // 定义方法RecordByDay()  按身份id 遍历添加
                        List listTemp = RecordByDay(resultSet.getString("lrxxcode"), newEightyElder);
                        System.out.println(obj.getLrxxcode());
//                        System.out.println(newEightyElder.getBeginTime());
                        System.out.println(newEightyElder.getFwjlkssj());
                        System.out.println(newEightyElder.getFwjljssj());
                        System.out.println(listTemp);
                        obj.setDayFrequency(listTemp);
                    }
                } else {
                    list.add("null");
                }
            } else {
                list.add("预编译失败");
            }

        } catch (Exception e) {
            list.add("JDBC读取失败");
            e.printStackTrace();
        } finally {
            // 释放资源
            if (connection != null) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (statement != null) {
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (resultSet != null) {
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }

//            return resultList;
        }
        Map resultMap = new HashMap();
        resultMap.put("list", resultList);
        return new BaseResult(200, "success", resultMap);
    }
```
</details>
                  
## 预编译 spring整合版本
                  
<details>
<summary>&#127808; spring整合版本 &#127808;</summary>


```java
    /**
     * 服务滚动工单 done
     *
     * @return
     */
    @PostMapping(value = "/test")
    @ResponseBody
    public List<Map<String, Object>> test(String code) {


        String sql = "select f.id, f.FWJLGDZT type, f.fwjllrxm serviceObject, f.fwjlorg organization, f.FWJLKSSJ startTime, f.FWJLKSDZ serviceAddress, f.FWJLFWNR content\n" +
                "from platform.fwjl f , sh_test_2.sys_dept d\n" +
                "WHERE f.FWJLDEPTID = d.org_code\n" +
//                "and d.Street = '" + code + "' \n" +
                "and d.Street = ? \n" +
//                "and fwjlkssj < CURTIME()  \n" +
                "and fwjlkssj < ?  \n" +
                "order by fwjlkssj desc \n" +
                "limit 30";
//        int count = jdbcTemplate.update(insertSql, new PreparedStatementSetter() {
//            @Override
//            public void setValues(PreparedStatement pstmt) throws SQLException {
//                pstmt.setObject(1, "name4");
//            }});
        Object[] obj = {code, "21312", "123123"};
        List<Map<String, Object>> list = jdbcTemplate.queryForList(sql, new Object[]{code, "2021-04-23"});
        SqlRowSet sqlRowSet = jdbcTemplate.queryForRowSet(sql, new Object[]{code, "2021-04-23"});
        while (sqlRowSet.next()) {
            System.out.println(sqlRowSet.getString("id"));
        }
        return list;
    }
```
</details>