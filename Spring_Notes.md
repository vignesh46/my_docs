# SPRING NOTES
## IOC - Inversion Of Control
Instead of creating objects manually by giving "new", Making it as configurable.

## DI - Dependency Injection
**product** object depend on **feature** object.

**feature** dependency will be injected instead of creating object with **new** keyword inside **product** object.
```json
{
	"product":{
		"productId" : 123,
		"feature" : {
			"Desc" : "easy to maintain"
		}
	}
}
```
Spring Container = Application context

Spring Bean = simple java object

## Code to load the applicationContext.xml and access the bean

```java
ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
Coach baseballCoach = context.getBean("baseballCoach", Coach.class);
```

## Dependency Injection
- **Constructor Injection**

```xml
<bean id = "fortune" class = "com.srv.springdemo.HappyFortumeService"/>

<bean id = "baseballCoach" class = "com.srv.springdemo.BaseballCoach">
	<constructor-arg ref="fortune"/>
</bean>
```
		
- **Setter Injection**

```xml
<bean id = "fortune" class = "com.srv.springdemo.HappyFortumeService"/>

<bean id = "cricketCoach" class = "com.srv.springdemo.CricketCoach">
	<!-- object injection  -->
	<property name="fortuneService" ref="fortune"></property>
	<!-- literal injection  -->
	<property name="email" value="cricket@abc.com"></property>
</bean>
```

## Load property file in applicationContext.xml and use it

```xml
<context:property-placeholder location="classpath:sport.properties"/>`

<bean id = "cricketCoach" class = "com.srv.springdemo.CricketCoach">
	<property name="fortuneService" ref="fortune"></property>
	<!-- <property name="email" value="cricket@abc.com"></property>  -->
	<property name="email" value="${email}"/>
</bean>
```


## Bean Scopes:

- Singleton (Default scope)
- Prototype

```xml
<bean id = "fortune" class = "com.srv.springdemo.HappyFortumeService"/>
<bean id = "baseballCoach" class = "com.srv.springdemo.BaseballCoach" scope = "prototype">
	<constructor-arg ref="fortune"/>
</bean>
```
- request
- session
- global-session

## Custom methods when Bean initialize and destory:

```xml
<bean id = "baseballCoach" class = "com.srv.springdemo.BaseballCoach" init-method="doMystartupStuff" destroy-method="doMyCleanupStuff"/>
```

- doMystartupStuff and doMyCleanupStuff are our custom methods which we written inside BaseballCoach class.
- Custom method can have any names
- The method can have any access modifier (public, protected, private)
- The method can have any return type. However, "void' is most commonly used. If you give a return type just note that you will not be able to capture the return value. As a result, "void" is commonly used.
- The method can not accept any arguments. The method should be no-arg.
- For "prototype" scoped beans, Spring does not call the destroy method

## Annotation :
Below line will scan all the annotations inside the "com.srv.springdemo" package.
```xml
<context:component-scan base-package="com.srv.springdemo"></context:component-scan>
```
```java
@Component("theTennisCoach") //on top of class
Coach tennisCoach = context.getBean("theTennisCoach", Coach.class);
```
If bean Id not given, then spring will use class name as bean id by lowering the first character of the class eg. tennisCoach.

special case of when BOTH the first and second characters of the class name are upper case, then the name is NOT converted.

In this case give bean name in component param and use it

## Autowiring Injection Types :
- Constructor Injection
```java
@Autowired
public TennisCoach(FortuneService fortuneService) {
	this.fortuneService = fortuneService;
}
```
- Setter Injection
```java
@Autowired
public void setFortuneService(FortuneService fortuneService) {
	System.out.println("CricketCoach : Inside setFortuneService()");
	this.fortuneService = fortuneService;
}

//Not only setter, method can have any names
@Autowired
public void anyMethod(FortuneService fortuneService) {
	System.out.println("CricketCoach : Inside anyMethod()");
	this.fortuneService = fortuneService;
}
```
- Field Injection

Spring achieve this by using Java Reflection
```java
@Autowired
private FortuneService fortuneService;
```

## Qualifier :
What if Interface has multiple implementations
```console
Caused by: org.springframework.beans.factory.NoUniqueBeanDefinitionException: No qualifying bean of type 'com.srv.springdemo.FortuneService' available: expected single matching bean but found 2: happyFortuneService,randomFortuneService
	at org.springframework.beans.factory.config.DependencyDescriptor.resolveNotUnique(DependencyDescriptor.java:220)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1285)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1227)
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:640)
	... 15 more
```
```java
//Field
@Autowired
@Qualifier("randomFortuneService")
private FortuneService fortuneService;

//setter
@Autowired
@Qualifier("randomFortuneService")
public void setFortuneService(FortuneService fortuneService) {
	System.out.println("CricketCoach : Inside setFortuneService()");
	this.fortuneService = fortuneService;
}

//Constructor
//NOTE: here you have to give it inside constructor param
@Autowired
public TennisCoach(@Qualifier("happyFortuneService") FortuneService fortuneService) {
	this.fortuneService = fortuneService;
}
```

## Inject properties file using Java annotations
Configure properties file

```java
@Value("${email}")
private String coachEmail;
```
## Scopes using Annotation :
```java
//Example
@Scope("prototype")
```
## Bean lifecycle methods using Annotation :
NOTE: For below stuffs we require javax-annotation-api jar

- Init method : **@PostConstruct**

Executed after construct and injecting beans

```java
@PostConstruct
public void doMyStartUpStuff() {		
	System.out.println("doMyStartUpStuff Method");		
}
```

- Destroy Method : **@PreDestroy**

Executed before destroying bean

```java
@PreDestroy
public void doMyCleanUpStuff() {
	System.out.println("doMyCleanUpStuff Method");
}
```

## Java config :
```java
@Configuration
@ComponentScan("com.srv.springdemo")//Optional
public class JavaConfig {

}

AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(JavaConfig.class);
```
## Manual Bean Declaration :
```java
@Bean
public FortuneService sadFortSev() {
	return new SadFortuneService();
}

@Bean
public Coach swimCoach() {
	return new SwimCoach(sadFortSev());
}
	
//Bean name will be method name
Coach tennisCoach = context.getBean("swimCoach", Coach.class);
```
## Property Injection :
```java
//On top of Java Config file
@PropertySource("classpath:sport.properties")

//In Class on top of field
@Value("${email}")
private String email;
```
## Spring MVC :
						MODEL		MODEL
WebBrowser -> Front Controller(Dispatcher Servlet) -> Controller -> View Template

**@controller** inherited from **@component**

**Sample MVC Controller :**
```java
@RequestMapping("/")
public String showPage() {
	
	/**
	 * this will construct /WEB-INF/view/main-menu.jsp
	 * Since we configured prefix and suffix in spring-mvc-demo-servlet
	 */
	return "main-menu";
}
```
## Model:
```java
@RequestMapping("/processform2")
public String modelExample(HttpServletRequest request, Model model) {
	String name = request.getParameter("studentName");
	model.addAttribute("message", name.toUpperCase());
	return "helloworld";
}
```

## To access message in JSP :
Message : ${message}



