---
layout:     post
title:      "代理模式"
subtitle:   ""
date:       2018-03-10 21:37:09
author:     "ZhengWeihao"
header-img: "imgs/post-bg-default.jpg"
tags:
    - Java
    - DesignPattern
---

代理模式
---

* 目的：

  - 通过代理方法执行业务以外的公共服务，业务代码专注于业务本身

* 静态代理：

  - ![simple-factory](/imgs/designPattern/proxy/static-proxy.png)
  - 优点：
    - 简单
  - 缺点：
    - 代理对象固定，不易于维护

* 动态代理：

* ![simple-factory](/imgs/designPattern/proxy/dynamic-proxy.png)

  - 优点：
    - ​
  - 缺点：
    - ​

* JDK代理：

  - Proxy.newProxyInstance(ClassLoader, Interface, InvocationHandler)

* CGLib代理：

  - enhancer.setSuperclass(Class)
  - enhancer.setCallback(MethodInterceptor)
  - enhancer.creat().invokeSuper(Object, Objects)

* JDK代理实现原理（字节码处理过程）：

  - 获取代理对象，并获取它的接口

  - JDK Proxy类重新生成一个新的类（须实现被代理类所实现的所有方法）

  - 动态生成Java代码，并由一定的逻辑代码去调用

  - 编译生成新的Java代码的class文件

  - 重新加载JVM中运行

  - 代码过程：

    - ```java
      byte[] bytes = ProxyGenerator.generatePoxyClass("$Proxy0", new Class[]{Person.class});
      FileOutputStream os = new FileOutStream("E:\\$Proxy0.class");
      os.write(bytes);
      os.close
      ```

      ​

    - ```java
      public interface InvocationHandler {
          public Object invoke(Object proxy, Method method, Object[] args) ;
      }

      public class ClassLoader extends ClassLoader {
      	private File classPathFile;
          
          public ClassLoader() {
              String classPath = ClassLoader.class.getResource("").getPath();
              classPathStr = new File(classPath);
          }
          
          @Override
          protected Class<?> findClass(String name) throws ClassNotFoundException {
              String className = ClassLoader.class.getPackage().getName() + "." + name;
              if(classPathFile != null)　{
                  File classFile = new File(classPathFile, name.replacell("\\.", ".") + className);
                  if(classFile.exists()) {
                      FileInputStream in = null;
                      ByteArrayOutputStream out = null;
                      
                      try {
                          in = new FileInputStream(classFile);
                          out = new ByteArrayOutputStream();
                          byte[] buff = new byte[1024];
                          int len;
                          while ((len = in.read(buff)) != -1) {
                              out.write(buff, 0, len);
                          }
                          return defineClass(classname, out.toByteArray(), 0, out.size)
                      } catch(Exception e) {
                          e.printStackTrace();
                      } finally {
                          in.close();
                          out.close();
                      }
                  }
              }
          }
      }

      public class Proxy {
          public static Object newProxyInstance(ClassLoader classLoader, Class<?> interfaces, InvocationHandler invocationHandler) {
              // 动态生成.java文件
              generateSrc(interfaces);
              
              // 输出.java文件
              String filePath = Proxy.class.getResource("").getPath();
              File f = new File(filePath + "$Proxy0.java");
              FileWriter fw = new FileWriter(f);
              fw.write(src);
              fw.flush();
              fw.close();
              
              // 编译.class文件
              JavaCompiler compiler = ToolProvider.getSystemJavaCompiler();
              StandardJavaFileManager manage = compiler.getStandardFileManager(null);
              Iterable iterable = manage.getJavaFileObjects(f);
              
              JavaCompiler.CompilationTask task = compilation.getTask(null, message, null);
              task.call();
              manage.close();
              
              // 加载.class文件
              Class proxyClass = classLoader.findClass("$Proxy0");
              Constructor c = proxyClass.getConstructor(InvocationHandler.class);
              f.delete();
              
              // 返回代理对象
              return c.newInstance(h);
          }
          
          private static String generateSrc(Class<?>[] interfaces) {
              StringBuffer sb = new StringBuffer();
              sb.append("package ...\r\n");
              String interfacesStr = null;// interfaces生成implements语句
              String importsStr = null;// interfaces生成import语句
              sb.append(importsStr);
              sb.append("public class $Proxy0 implements" + interfacesStr + "\r\n");
              sb.append("InvocationHandler h;");
              sb.append("public $Proxy0(InvocationHandler h) {this.h = h}");
              for(Method m : interfaces[0].getMethods()) {
                  sb.append("...");// 生成方法体
              }
              return null;
          }
      }
      ```

* 应用场景：

  * 日志记录

