# SPRING NOTES
## IOC - Inversion Of Control
Insted of creating objects manually by giving "new", Making it as configurable.

	
Spring Container = Application context

Spring Bean = simple java object

Code to load the **applicationContext.xml** and access the bean

`ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
Coach baseballCoach = context.getBean("baseballCoach", Coach.class);`

## Dependency Injection
- **Constructor Injection**

`<bean id = "fortune" class = "com.srv.springdemo.HappyFortumeService"/>
<bean id = "baseballCoach" class = "com.srv.springdemo.BaseballCoach">
	<constructor-arg ref="fortune"/>
</bean>`
		
- **Setter Injection**
`<bean id = "fortune" class = "com.srv.springdemo.HappyFortumeService"/>
<bean id = "cricketCoach" class = "com.srv.springdemo.CricketCoach">
<!-- object injection  -->
<property name="fortuneService" ref="fortune"></property>
<!-- literal injection  -->
<property name="email" value="cricket@abc.com"></property>
</bean>`

## Load property file in applicationContext.xml and use it

`<context:property-placeholder location="classpath:sport.properties"/>

<bean id = "cricketCoach" class = "com.srv.springdemo.CricketCoach">
	<property name="fortuneService" ref="fortune"></property>
	<!-- <property name="email" value="cricket@abc.com"></property>  -->
	<property name="email" value="${email}"/>
</bean>`


## Bean Scopes:

- Singleton (Default scope)
- Prototype

`<bean id = "fortune" class = "com.srv.springdemo.HappyFortumeService"/>
<bean id = "baseballCoach" class = "com.srv.springdemo.BaseballCoach" scope = "prototype">
<constructor-arg ref="fortune"/>
</bean>`

- request
- session
- global-session

## Custom methods when Bean initialize and destory:

`<bean id = "baseballCoach" class = "com.srv.springdemo.BaseballCoach" init-method="doMystartupStuff" destroy-method="doMyCleanupStuff"/>`

- doMystartupStuff and doMyCleanupStuff are our custom methods which we written inside BaseballCoach class.
- Custom method can have any names
- The method can have any access modifier (public, protected, private)
- The method can have any return type. However, "void' is most commonly used. If you give a return type just note that you will not be able to capture the return value. As a result, "void" is commonly used.
- The method can not accept any arguments. The method should be no-arg.
- For "prototype" scoped beans, Spring does not call the destroy method

