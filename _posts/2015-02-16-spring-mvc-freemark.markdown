---
layout: index
title: SpringMVC集成Freemark及JSON格式输出
date: 2015-02-16 12:59:39
categories: [backend, java, spring, springMVC]
---

SpringMVC集成Freemark及JSON格式输出
---
> 最近做项目需要使用SpringMVC代替Struts2，因此进行了对SpringMVC的一些初步学习，而在以前的项目中，也是对Freemark比较熟悉，所以也做了整合Freemark的一些工作，在这里做个记录。

### 第一部分：SpringMVC基本配置 ###

* Jar包：

    项目为Maven工程，所以使用pom文件管理jar包，具体依赖如下：

    	  <!--spring 依赖 -->
    	  <dependency>
    	    <groupId>org.springframework</groupId>
    	    <artifactId>spring-beans</artifactId>
    	    <version>3.1.0.RELEASE</version>
    	  </dependency>
    	  <dependency>
    		<groupId>org.springframework</groupId>
    		<artifactId>spring-context</artifactId>
    		<version>3.1.0.RELEASE</version>
    	  </dependency>
    	  <dependency>
    		<groupId>org.springframework</groupId>
    		<artifactId>spring-context-support</artifactId>
    		<version>3.1.0.RELEASE</version>
    	  </dependency>
    	  <dependency>
    		<groupId>org.springframework</groupId>
    		<artifactId>spring-core</artifactId>
    		<version>3.1.0.RELEASE</version>
    	  </dependency>
    	  <dependency>
    	    <groupId>org.springframework</groupId>
    	    <artifactId>spring-aop</artifactId>
    	    <version>${spring.version}</version>
    	  </dependency>
    	  <dependency>
    		<groupId>org.springframework</groupId>
    		<artifactId>spring-web</artifactId>
    		<version>3.1.0.RELEASE</version>
    	  </dependency>
    	  <dependency>
    	    <groupId>org.springframework</groupId>
    	    <artifactId>spring-webmvc</artifactId>
    	    <version>${spring.version}</version>
    	  </dependency>
    	  <dependency>
    	    <groupId>org.springframework</groupId>
    	    <artifactId>spring-tx</artifactId>
    	    <version>${spring.version}</version>
    	  </dependency>
    	  <dependency>
    	    <groupId>org.springframework</groupId>
    	    <artifactId>spring-orm</artifactId>
    	    <version>${spring.version}</version>
    	  </dependency>
    	  <dependency>
    	    <groupId>org.springframework</groupId>
    	    <artifactId>spring-jdbc</artifactId>
    	    <version>${spring.version}</version>
    	  </dependency>
    	  
    	  <!-- Freemarker -->
    	  <dependency>
    	    <groupId>org.freemarker</groupId>
    	    <artifactId>freemarker</artifactId>
    	    <version>2.3.20</version>
    	  </dependency>

* Spring支持与SpringMVC基本配置：

    １、Spring基本配置，在Web.xml中添加相应地监听器，并指定配置文件（记得建立配置文件，否则会报FileNotFound异常），如下：

    	<!-- Spring 上下文配置文件 -->
    	<context-param>
    		<param-name>contextConfigLocation</param-name>
    		<param-value>classpath:applicationContext.xml</param-value>
    	</context-param>
    	
    	<!-- Spring 刷新Introspector防止内存泄露 -->
    	<listener>
    		<listener-class>
    			org.springframework.web.util.IntrospectorCleanupListener
    		</listener-class>
    	</listener>
    	
    	<!-- Spring 使用ContextLoaderListener加载ApplicationContext配置信息 -->
    	<listener>
    		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    	</listener>


	　　２、SpringMVC配置(Web.xml中配置，并建立spring-mvc.xml配置文件）。

	　　在web.xml中添加DispatcherServlet的设定，并设置url-pattern为要捕获的路径规则（也可设置成类似*.action的格式），如下：

		<!-- Spring MVC -->
		<servlet>
			<servlet-name>springDispatcherServlet</servlet-name>
			<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
			<init-param>
				<param-name>contextConfigLocation</param-name>
				<param-value>classpath:spring-mvc.xml</param-value>
			</init-param>
			<load-on-startup>1</load-on-startup>
		</servlet>
		<servlet-mapping>
			<servlet-name>springDispatcherServlet</servlet-name>
			<url-pattern>/</url-pattern>
		</servlet-mapping>
	
	　　在spring-mvc.xml中配置页面返回规则，并指定Spring自动扫描的路径，如下：
	
		<!-- Spring MVC UrlMapping -->
				<!-- Spring MVC UrlMapping -->
		<bean id="urlMapping"
			class="org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping" />
		<bean
			class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">
			<property name="messageConverters">
				<list>
					<bean id="mappingStringHttpMessageConverter" class="org.springframework.http.converter.StringHttpMessageConverter">
						<property name="supportedMediaTypes">
							<list>
								<bean class="org.springframework.http.MediaType">
									<constructor-arg index="0" value="text" />
									<constructor-arg index="1" value="plain" />
									<constructor-arg index="2" value="UTF-8" />
								</bean>
							</list>
							<!-- <list>  
				                <value>text/plain;charset=UTF-8</value>  
				                <value>text/html;charset=UTF-8</value>  
				            </list> -->
						</property>
					</bean>
				</list>
			</property>
		</bean>
		
		<!-- 自动扫描转化 标有@Controller注解的类为bean -->
		<context:component-scan base-package="com.test" />
	
	　　３、编写控制器，以供测试，控制器代码如下：
	
		@Controller
		public class TestController {
	
			@RequestMapping("/test_index")
			public ModelAndView index(HttpServletRequest req) {
				ModelAndView mav = new ModelAndView("index");
				mav.addObject("pix", pix);
				return mav;
			}
		}



### 第二部分：SpringMVC集成Freemark输出模板页面 ###

* 添加Freemark的pom依赖，如下：
    ​	
    <!-- Freemarker -->
      <dependency>
        <groupId>org.freemarker</groupId>
        <artifactId>freemarker</artifactId>
        <version>2.3.20</version>
      </dependency>

* 在spring-mvc.xml中设置使用Freemark为视图解析器，如下：

   <!-- FreeMarker基础支持 -->  
       <bean id="freemarkerConfigurer" class="org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer">  
           <property name="templateLoaderPath" value="/view/" />
           <property name="defaultEncoding" value="UTF-8" />
           <property name="freemarkerSettings">
               <props>
                   <prop key="template_update_delay">10</prop>
                   <prop key="locale">zh_CN</prop>
                   <prop key="datetime_format">yyyy-MM-dd HH:mm:ss</prop>
                   <prop key="date_format">yyyy-MM-dd</prop>
                   <prop key="number_format">#.##</prop>
               </props>
           </property>
       </bean>
       <!-- FreeMarker视图解析 -->
       <bean id="viewResolver" class="org.springframework.web.servlet.view.freemarker.FreeMarkerViewResolver">  
           <property name="viewClass" value="org.springframework.web.servlet.view.freemarker.FreeMarkerView" />
           <property name="suffix" value=".ftl" />
           <property name="contentType" value="text/html;charset=UTF-8" />
           <property name="exposeRequestAttributes" value="true" />
           <property name="exposeSessionAttributes" value="true" />
           <property name="exposeSpringMacroHelpers" value="true" />
       </bean>

* 控制器中进行测试

   @RequestMapping("/test_ftl")
   	public ModelAndView index(HttpServletRequest req) {
   		String pix = req.getParameter("pix");
   		
   		ModelAndView mav = new ModelAndView("index");
   		mav.addObject("pix", pix);
   		return mav;
   	}



### 第三部分：配置SpringMVC输出JSON格式的数据 ###

* 在pom文件中添加对Jackson的依赖，如下：

     <!-- JSON for Spring MVC -->
     	  <dependency>
     		<groupId>org.codehaus.jackson</groupId>
     		<artifactId>jackson-core-asl</artifactId>
     		<version>1.8.0</version>
     	  </dependency>
     	  <dependency>
     		<groupId>org.codehaus.jackson</groupId>
     		<artifactId>jackson-mapper-asl</artifactId>
     		<version>1.9.7</version>
     	  </dependency>
     	  <dependency>
     		<groupId>org.codehaus.jackson</groupId>
     		<artifactId>jackson-jaxrs</artifactId>
     		<version>1.8.0</version>
     	  </dependency>
     	  <dependency>
     		<groupId>org.codehaus.jackson</groupId>
     		<artifactId>jackson-xc</artifactId>
     		<version>1.8.0</version>
     	  </dependency>

* 在spring-mvc.xml中配置JSON格式的支持，如下：

   <!-- Spring MVC JSON输出格式 -->
   	<bean id="mappingJacksonHttpMessageConverter" class="org.springframework.http.converter.json.MappingJacksonHttpMessageConverter">
   		<property name="supportedMediaTypes">
   			<list>
   				<value>application/json;charset=UTF-8</value>
   				<value>text/html;charset=UTF-8</value>
   			</list>
   		</property>
   	</bean>

   并将Jackson的实体引入到AnnotationMethodHandlerAdapter类的messageConverters属性当中，如下：

   	<bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">
   		<property name="messageConverters">
   			<list>
   				<!-- JSON文本输出格式 -->
   				<ref bean="mappingJacksonHttpMessageConverter" />
   				<!-- 简单文本输出格式 -->
   				<ref bean="mappingStringHttpMessageConverter" />
   			</list>
   		</property>
   	</bean>

* 在控制器中编写JSON格式测试方法，如下：

   @RequestMapping("/test_json")
   	@ResponseBody
   	public JSONObject testJson(Long id, String name) {
   		JSONObject obj = new JSONObject();
   		obj.put("id", id);
   		obj.put("name", name);
   		return obj;
   	}


