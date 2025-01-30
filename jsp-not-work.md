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

### **Intermittent Issues and Their Solutions**

1. **Maven Dependencies Not Properly Loaded**

   - **Issue:** Sometimes, dependencies may not be resolved properly, leading to `ClassNotFoundException`.
   - **Solution:** Run:
     ```sh
     mvn clean
     mvn install
     ```
     This will ensure all dependencies are reloaded correctly.

2. **Cached JSP Files Causing Errors**

   - **Issue:** Tomcat may cache older JSP files, causing inconsistencies.
   - **Solution:** Try deleting the `target/` folder and restart the application:
     ```sh
     mvn clean package
     ```

3. **Java Version Mismatch**

   - **Issue:** Java version conflicts, especially if you are using a different JDK than specified in `pom.xml`.
   - **Solution:** Verify Java version with:
     ```sh
     java -version
     ```
     Ensure it matches the version in `pom.xml` (Java 21 in your case).

4. **Missing or Incorrect Deployment of JSP Files**

   - **Issue:** JSP files may not be correctly placed in `src/main/webapp/views/`.
   - **Solution:** Ensure that all JSP files exist and match the configurations in `application.properties`:
     ```properties
     spring.mvc.view.prefix=/views/
     spring.mvc.view.suffix=.jsp
     ```

5. **Port Conflict**

   - **Issue:** If another application is using port `8080`, Spring Boot may fail to start.
   - **Solution:** Run the application on a different port by adding to `application.properties`:
     ```properties
     server.port=8081
     ```

6. **DevTools Hot Reload Issues**

   - **Issue:** Spring Boot DevTools may not always refresh JSP changes automatically.
   - **Solution:** Try disabling caching:
     ```properties
     spring.thymeleaf.cache=false
     ```

7. **Check Logs for Specific Errors**

   - Run:
     ```sh
     mvn spring-boot:run
     ```
     and check the logs for errors. If you see `org.apache.jasper.JasperException`, it indicates an issue with the JSP compilation.

8. **Use Embedded Tomcat Correctly**
   - **Issue:** JSP requires `tomcat-jasper`. If missing, JSP will not compile.
   - **Solution:** Ensure the following dependency is included:
     ```xml
     <dependency>
         <groupId>org.apache.tomcat</groupId>
         <artifactId>tomcat-jasper</artifactId>
         <version>10.1.16</version>
     </dependency>
     ```

If the problem persists, try **rebuilding the project, restarting your IDE, and running the application from the command line instead of the IDE**.

### **Common Errors and Solutions**

| **Error**                                                       | **Solution**                                                         |
| --------------------------------------------------------------- | -------------------------------------------------------------------- |
| `Whitelabel Error Page`                                         | Ensure `home.jsp` exists inside `src/main/webapp/views/`             |
| `JSP files not found`                                           | Check `spring.mvc.view.prefix` and `spring.mvc.view.suffix` settings |
| `ClassNotFoundException: org.apache.jasper.servlet.JspServlet`  | Ensure `tomcat-jasper` dependency is included                        |
| `JSTL tags not working`                                         | Ensure correct JSTL dependencies are added                           |
| `Application fails with java.lang.UnsupportedClassVersionError` | Ensure your Java version is compatible (Java 21 in `pom.xml`)        |

---
