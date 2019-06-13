---
layout:     post
title:      "模板模式"
subtitle:   ""
date:       2018-03-11 20:57:34
author:     "ZhengWeihao"
header-img: "imgs/post-bg-default.jpg"
tags:
    - Java
    - DesignPattern
---

模板模式
---

* 目的：
  * 复用固有流程，传入个性化设置
* UML图：
  * ![simple-factory](/imgs/designPattern/template/template.png)
* 应用：
  * JdbcTemplate：

    * ```java
      public class JDBCTemplate {
          
          private DataSource dataSource;
          public JDBCTemplate(DataSource dataSource) {
              this.dataSource = dataSource;
          }
          
          private Connection getConnection() {
              return this.dataSource.getConnection();
          }
          
          private PreparedStatement createPreparedStatement(Connection conn, String sql) {
              return conn.prepareStatement(sql);
          }
          
          private ResultSet executeQuery(PreparedStatement pstmt, Object[] params) {
              for (int i=0; i<params.length; i++) {
                  Object param = params[i];
                  pstmt.setObject(i, param);
              }
              return pstmt;
          }
          
          private List<?> parseResultSet(ResultSet rs, rowMapper rowMapper) {
              List<?> result = new ArrayList<Object>();
              rowNum = 0;
              while (rs.hasNext) {
                  result.add(rowMapper.mapROw(rs.));
              }
              return result;
          }
          
          public List<?> executeQuery(String sql, RowMapper<?> rowMapper, Object[] params)　{
              // 获取连接
              Connection conn = this.getConnection();

              // 创建语句集
              PreparedStatement pstmt = createPreparedStatement(conn, sql);

              // 执行语句集，并且获得结果集
              ResultSet rs = executeQuery(pstmt, params);

              // 解析语句集
              List<?> result = this.parseResultSet(rs, rowMapper);

              // 关闭结果集、语句集、连接
              rs.close();
              pstmt.close();
              conn.close();
              
              return result;
          }

          public Object processResult(ResultSet rs, int rowNum) {

          }
      }

      public class MemberDao {
          private dataSource = null;
          private JDBCTemplate = new JDBCTemplete(dataSource);
          
          public MemberDao(DataSource dataSource) {
              this.dataSource = dataSource;
          }
          
          public List<Object> query() {
              String sql = "...";
              
          }
      }

      public interface RowMapper<T> {
          public T mapRow(ResultSet rs, int rowNum);
      }
      ```

      ​
* 补充：
  * ​

