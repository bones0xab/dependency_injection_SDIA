# Java Dependency Injection Overview

This project explores multiple approaches to implementing Dependency Injection (DI) in Java, demonstrating both traditional and Spring-based methods.

## Table of Contents

- [Introduction](#introduction)
- [Project Structure](#project-structure)
- [Dependency Injection Approaches](#dependency-injection-approaches)
  - [Static Coupling](#static-coupling)
  - [Dynamic Instantiation](#dynamic-instantiation)
  - [Spring XML Configuration](#spring-xml-configuration)
  - [Spring Annotations](#spring-annotations)
- [Dependencies](#dependencies)
- [Execution Guide](#execution-guide)

## Introduction

Dependency Injection (DI) is a key principle in software design that improves modularity and maintainability by decoupling components. This project covers:

1. **Static Coupling**: Direct instantiation of dependencies.
2. **Dynamic Instantiation**: Using reflection and configuration files.
3. **Spring XML Configuration**: DI using XML-based bean configuration.
4. **Spring Annotations**: Modern annotation-driven DI with Spring.

## Project Structure

```
dependency-injection/
├── src/main/java/com/jee/
│   ├── static_instanciation/
│   ├── dynamic_instanciation/
│   ├── spring/
│   │   ├── xml/
│   │   └── annotations/
│   └── resources/
└── config files (config.xml, config.yaml, config.txt)
```

## Dependency Injection Approaches

### Static Coupling

A basic implementation where dependencies are directly instantiated in the code.

```java
IDao dao = new IDaoImpl();
IMetier metier = new MetierImpl(dao);
System.out.println(metier.calcul());
```

### Dynamic Instantiation

Dynamically loads dependencies using reflection based on configuration files.

#### Example Configuration (YAML)
```yaml
classes:
  dao: "com.jee.dynamic_instanciation.dao.IDaoImpl"
  metier: "com.jee.dynamic_instanciation.metier.MetierImpl"
```

#### Java Reflection-Based Instantiation
```java
IDao dao = (IDao) Class.forName(daoClassName).getConstructor().newInstance();
IMetier metier = (IMetier) Class.forName(metierClassName).getConstructor().newInstance();
Method method = metier.getClass().getDeclaredMethod("setDao", IDao.class);
method.invoke(metier, dao);
System.out.println(metier.calcul());
```

### Spring XML Configuration

Dependency Injection using Spring's XML-based configuration.

#### XML Configuration
```xml
<beans>
    <bean id="dao" class="com.jee.spring.xml.DaoImpl"/>
    <bean id="metier" class="com.jee.spring.xml.MetierImpl">
        <constructor-arg ref="dao"/>
    </bean>
</beans>
```

#### Java Implementation
```java
ApplicationContext context = new ClassPathXmlApplicationContext("config.xml");
IMetier metier = context.getBean(IMetier.class);
System.out.println(metier.calcul());
```

### Spring Annotations

Uses annotation-based configuration for Dependency Injection.

```java
ApplicationContext context = new AnnotationConfigApplicationContext("com.jee.spring.annotations");
IMetier metier = context.getBean(IMetier.class);
System.out.println(metier.calcul());
```

## Dependencies

- Spring Core 6.2.3
- Spring Context 6.2.3
- Spring Beans 6.2.3
- Jakarta Annotation API 2.1.1

## Execution Guide

1. **Clone the repository**
2. **Build with Maven:**

```bash
mvn clean install
```

3. **Run the main classes:**
   - **Static Implementation:** `com.jee.static_instanciation.presentation.Presentation`
   - **Dynamic Implementation:** `com.jee.dynamic_instanciation.presentation.Presentation`
   - **Spring XML:** `com.jee.spring.xml.presentation.Presentation`
   - **Spring Annotations:** `com.jee.spring.annotations.presentation.Presentation`

