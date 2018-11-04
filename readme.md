
Reference
==

https://www.journaldev.com/8934/spring-rest-xml-and-json-example


- Define a bean of type Jaxb2RootElementHttpMessageConverter.

```xml
<beans:bean id="xmlMessageConverter" class="org.springframework.http.converter.xml.Jaxb2RootElementHttpMessageConverter">
</beans:bean>

```


- Add above configured bean to RequestMappingHandlerAdapter property messageConverters.

```xml
<beans:bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
<beans:property name="messageConverters">
	<beans:list>
		<beans:ref bean="jsonMessageConverter"/>
		<beans:ref bean="xmlMessageConverter"/>
	</beans:list>
</beans:property>
</beans:bean>
```

- put  servlet-context.xml into classpath

```xml

<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

	<!-- DispatcherServlet Context: defines this servlet's request-processing infrastructure -->
	
	<!-- Enables the Spring MVC @Controller programming model -->
	<annotation-driven />

	<!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory -->
	<resources mapping="/resources/**" location="/resources/" />

	<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
	
	<!-- Configure to plugin JSON as request and response in method handler -->
	<beans:bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
		<beans:property name="messageConverters">
			<beans:list>
				<beans:ref bean="jsonMessageConverter"/>
				<beans:ref bean="xmlMessageConverter"/>
			</beans:list>
		</beans:property>
	</beans:bean>
	
	<!-- Configure bean to convert JSON to POJO and vice versa -->
	<beans:bean id="jsonMessageConverter" class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
	</beans:bean>	
	
	<beans:bean id="xmlMessageConverter" class="org.springframework.http.converter.xml.Jaxb2RootElementHttpMessageConverter">
	</beans:bean>
	
	<context:component-scan base-package="com.journaldev.spring.controller" />
	
</beans:beans>
```

- We know that for working with JAXB marshalling for a class, we need to annotate it with @XmlRootElement annotation. So add this to our Employee model class.

```java

@XmlRootElement
public class Employee implements Serializable{

//no change in code
}

```

That’s it, we are DONE. Our Spring application will support both JSON as well as XML. It will even support XML request with JSON response and vice versa. Below are some of the screenshots showing this in action.

NOTE: I am using Postman Chrome application for this, you can use any rest client for this testing.
