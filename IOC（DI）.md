## IOC（DI）

## 1. IOC

> **`Inverse Of Controll`:控制反转**
>
> **反转了依赖关系的满足方式，由之前的自己创建依赖对象，变为由工厂推送。(变主动为被动，即反转)**
>
> **解决了具有依赖关系的组件之间的强耦合，使得项目形态更加稳健**

## 2. DI

> **`Dependency Injection`：依赖注入**
>
> **全新的依赖满足方式，体现在编码中就是全新的赋值方式 ==> `在工厂中为属性推送值`**
>
> **如：**`<property name="userDAO" ref="userDAO"></property>`

## 3. IOC 和 DI

> **在spring中关于IOC和DI的描述是这样的:** `IOC(DI)`**，即，是一码事**
>
> `IOC 是思想`**：指导我们在满足依赖时，应该有反转的设计。**
>
> `DI 是手段`**：实际操作时，就是在一次次的 注入**

> **spring官方文档中，关于[DI](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-collaborators)的解释：**
>
> `This process is fundamentally the inverse (hence the name, Inversion of Control) of the bean itself controlling the instantiation or location of its dependencies on its own by using direct construction of classes or the Service Locator pattern`

## 4.注入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--  声明，哪些组件需要生产  -->
    <!-- 当spring启动时，读取配置文件，创建对应类的对象，并用id做标识 -->
    <!-- userDao被自动注入给AutoComponent的userDao -->
    <bean id="userDao" class="com.qf.dao.UserDaoImpl"></bean>

    <!--  userService 中 userdao 需要赋值  -->
    <bean id="userService" class="com.qf.service.UserServiceImpl">
        <property name="userDao" ref="userDao"/>
    </bean>
    <!--  注入测试  -->
    <!--  set注入  -->
    <bean id="di" class="com.qf.di.SetComponent">
        <!--  简单类型 基本类型+String -->
        <property name="id" value="4"/>
        <property name="name" value="亚辉"/>
        <property name="gender" value="true"/>
        <!--  引用类型  -->
        <property name="userDao" ref="userDao"/>
        <!--  list  -->
        <property name="list">
            <list>
                <value>gp04</value>
                <ref bean="userDao"></ref>
            </list>
        </property>
        <!--  set  -->
        <property name="set">
            <set>
                <value>辉辉</value>
                <value>坤坤</value>
                <value>晓虎</value>
                <value>晓虎</value>
            </set>
        </property>
        <!--  map  -->
        <property name="map">
            <map>
                <entry key="username" value="suxing"/>
                <entry key="userdao" value-ref="userDao"/>
            </map>
        </property>
        <!--  Properties  -->
        <property name="properties">
            <props>
                <prop key="url">jdbc:mysql//xxx</prop>
                <prop key="driver">com.cj.jdbc</prop>
            </props>
        </property>
    </bean>
    
    <!--  构造注入 -->
    <bean id="consDI" class="com.qf.di.ConsComponent">
        <constructor-arg index="0" type="java.lang.Integer" value="4"></constructor-arg>
        <constructor-arg index="1" type="java.lang.String" value="少泊"></constructor-arg>
        <constructor-arg index="2" type="java.lang.Boolean" value="false"></constructor-arg>
    </bean>
    
    <!--  自动注入测试
          autowire = 自动装载
          byType = 将工厂中和属性类型一致的组件，赋值给对应属性
          基于类型自动注入时，如果存在多个该类型的bean，会报错
          byName = 将工厂中和属性名称一致的组件，赋值给对应属性
    -->
    <bean id="autoDI" class="com.qf.di.AutoComponent" autowire="byType"/>

    <!--  AutoComponent的成员变量UserDao userDao 被自动注入
		  userDao需要有set方法才能被自动注入
	-->
    
    <!--  工厂bean
          细节: 1. factorybean的生命周期和普通的bean没区别
               2. 当用户通过getbean 获得一个factory时 返回的不是工厂bean本身，而是其生产的对象
               3. 如果获得工厂bean的本体 需要 '&beanID' = '&mySqlSessionFactory'
    -->
    <bean id="mySqlSessionFactory" class="com.qf.factory.MySqlSessionFactoryBean">

    </bean>
</beans>
```