**Project Structure**

```
JobApp-Project/
│-- src/
│   ├── main/
│   │   ├── java/com/telusko/JobApp/
│   │   │   ├── controller/
│   │   │   │   ├── JobController.java
│   │   │   ├── model/
│   │   │   │   ├── JobPost.java
│   │   │   ├── repo/
│   │   │   │   ├── JobRepo.java
│   │   │   ├── service/
│   │   │   │   ├── JobApplication.java
│   │   ├── resources/
│   │   │   ├── application.properties
│   │   ├── webapp/
│   │   │   ├── views/
│   │   │   │   ├── home.jsp
│   │   │   │   ├── addjob.jsp
│   │   │   │   ├── success.jsp
│   │   │   │   ├── viewalljobs.jsp
│   │   │   ├── style.css
│-- pom.xml
│-- mvnw
│-- mvnw.cmd
│-- .gitignore
```

**Steps to Run Spring Boot with JSP Successfully**

### **1. Ensure Proper Dependencies in `pom.xml`**

Spring Boot 3 uses Jakarta EE instead of Java EE, so dependencies must be correctly added.

#### **Required Dependencies:**

```xml
<dependencies>
    <!-- Spring Boot Web Starter -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- JSP Support via Tomcat Jasper -->
    <dependency>
        <groupId>org.apache.tomcat</groupId>
        <artifactId>tomcat-jasper</artifactId>
        <version>10.1.16</version>
    </dependency>

    <!-- JSTL Support -->
    <dependency>
        <groupId>jakarta.servlet.jsp.jstl</groupId>
        <artifactId>jakarta.servlet.jsp.jstl-api</artifactId>
    </dependency>
    <dependency>
        <groupId>org.glassfish.web</groupId>
        <artifactId>jakarta.servlet.jsp.jstl</artifactId>
    </dependency>

    <!-- Lombok for reducing boilerplate code -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>

    <!-- Spring Boot DevTools for live reload -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <scope>runtime</scope>
        <optional>true</optional>
    </dependency>

    <!-- Testing dependencies -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### **2. Configure `application.properties` for JSP View Resolver**

```properties
spring.mvc.view.prefix=/views/
spring.mvc.view.suffix=.jsp
```

Ensure your `JSP` files are inside `src/main/webapp/views/`.

### **3. Set Up the Controller**

Create `JobController.java` under `com.telusko.JobApp.controller`.\
This is demo code replace with Github code of Job portal.

```java
package com.telusko.JobApp.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class JobController {
    @GetMapping("/home")
    public String home() {
        return "home"; // home.jsp
    }
}
```

### **4. Ensure `home.jsp` Exists in `src/main/webapp/views/`**

Create `home.jsp` inside `src/main/webapp/views/`.

This is demo code replace with Github code of Job portal.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<html>
<head>
    <title>Home Page</title>
</head>
<body>
    <h1>Welcome to the Job Portal</h1>
</body>
</html>
```

### **5. Adjust `src/main/webapp/WEB-INF/web.xml` (If Required)**

Spring Boot does not require `web.xml`, but if using an older setup, define a JSP dispatcher:

```xml
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
    version="3.0">

    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

### **6. Run the Application**

```sh
mvn spring-boot:run
```

OR

```sh
mvn clean install
java -jar target/JobApp-0.0.1-SNAPSHOT.jar
```

### **7. Access in Browser**

- Run `http://localhost:8080/home` to verify the JSP page loads correctly.

### **Common Errors and Solutions**

| **Error**                                                       | **Solution**                                                         |
| --------------------------------------------------------------- | -------------------------------------------------------------------- |
| `Whitelabel Error Page`                                         | Ensure `home.jsp` exists inside `src/main/webapp/views/`             |
| `JSP files not found`                                           | Check `spring.mvc.view.prefix` and `spring.mvc.view.suffix` settings |
| `ClassNotFoundException: org.apache.jasper.servlet.JspServlet`  | Ensure `tomcat-jasper` dependency is included                        |
| `JSTL tags not working`                                         | Ensure correct JSTL dependencies are added                           |
| `Application fails with java.lang.UnsupportedClassVersionError` | Ensure your Java version is compatible (Java 21 in `pom.xml`)        |

### **Conclusion**

By following these steps, your Spring Boot JSP application should run smoothly. If issues persist, check dependency versions, logs, and ensure that JSP files are correctly placed inside `src/main/webapp/views/`.
