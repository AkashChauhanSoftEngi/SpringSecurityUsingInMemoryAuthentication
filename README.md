# SpringSecurityUsingInMemoryAuthentication

* Spring Framework + Spring Security + Java Configuration + MVC + Maven , Example
* Spring Security example for Spring 4 MVC + JSP View with pure Java Configuration (no XML), using Maven build tool.
* Spring4 + MVC, Integration without using the web.xml and Spring_Servlet.xml file. 
* By using WebMvcConfigurerAdapter class and WebApplicationInitializer interface to replace above files.

> **###1. Technologies**
* Spring 4.0.3.RELEASE
* Spring Security 3.2.5.RELEASE {spring-security-web, spring-security-config, spring-security-taglibs}
* Maven 3.1
* JSTL 1.2

> **###2. To Run this project locally**
* $ git clone https://github.com/AkashChauhanSoftEngi/SpringSecurityUsingInMemoryAuthentication
* $ mvn tomcat7:run

> **###3.  Access** 
* http://localhost:8080/SpringSecurityUsingInMemoryAuth/Spring/
* http://localhost:8080/SpringSecurityUsingInMemoryAuth/Spring/user
* http://localhost:8080/SpringSecurityUsingInMemoryAuth/Spring/admin

> **###4.  Important Points to keep in mind**
* Spring Security is a web authentication and authorization framework for Java Servlet web applications or REST services
* It installs a chain of servlet filters in front of the application’s filters and servlets[DS], at startup
* A another similar framework available at Apache Shiro
* Add these jars 	
* Need to create a class to extend AbstractSecurityWebApplicationInitializer
* Spring Security is useable to confirm the points a) and b) 
```diff
+ Need to create a another class[p] which extends WebSecurityConfigurerAdapter and override its two overloaded methods
  a)- void configure(AuthenticationManagerBuilder): to confirm if user belongs to the database
  b)- void configure(HttpSecurity): to confirm if the user is allowed to access the resources/end points
```
* AuthenticationManagerBuilder.inMemoryAuthentication()....
* HttpSecurity.authorizeRequests().antMatchers("/").permitAll()...
* this class[p], needs to be registered as RootConfigClass in web initilizer class
* Always make sure to use compatible versions of spring and spring security [exa: S(4.3.0.RELEASE) & SS(3.2.5.RELEASE)]
```diff
+ Spring-Security creates new servlet(filter) like Dispature servlet, such filters will be created before Dispature servlet though, which confirm first points a) and b) and if all okay then, let the flow move to Dispature servlet.
```
> **###5. JSP Communication with DS,Controller**
* From JSP page in given project, it is checking security concerns by using these 
  - access="isAnonymous()"
  - access="sAuthenticated()"
  - access="hasRole('USER')"
  - To confirm Authentication and Autherization of the user, it will forward to a predefined "LoginPage" and then will check the credentials from the stroed Configuraion Builders like HttpSecurity and AuthenticationManagerBuilder

> **###6. Work Flow**
* web->Security Filter->Filters if any + Web App filter if any->DispatcherServlet->(Controllers + JSPs)
* Before even coming to DispatcherServlet, controller or any page, security concerns will pass through FilterChainProxy.doFilter() filter.
	
> **###7. How Spring Security communicate with DispatcherServlet**
* Before using DispatcherServlet filters, the flow has to go through security filters like (FilterChainProxy.doFilter())
* And as the job of the DispatcherServlet is to take an incoming URI and find the right combination of handlers (generally methods on Controller classes) and views (generally JSPs) that combine to form the page or resource that's supposed to be found at that location.
* One way to do that is: 
  a) Need to override an method startUp() from WebApplicationInitializer interface
 	- Create a context
	- Use this context to register all configuration files (@Configuraion files)
	- Then creating and adding a dynamic servlet, and then pass this context as parameter
	- Then you can set the priority of the filter
	- You can use addMapping("URL") to map incoming url{to bind what kind of URL this filter will pass[example: "/*"]}
