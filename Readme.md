<h1>Introduction</h1>
Demo web application demonstrating some of the key features of Java EE 7 and Spring MVC 3.2, including form/bean validations, Spring's exception handling, building RESTful JSON services, and web application initilization using JavaConfig (no Xml at all). There are no static pages for the web application, the focus is on building RESTful JSON services. Future enhancements to include WebSockets 1.0, as well as JMS 2.0 integration with Spring.

<h2>Outline</h2>

1. Java EE 7: Bean validation using JSR(349) reference implementation Hibernate
2. Spring MVC 3.2 Exception handling using @ControllerAdvice annotation
3. Validate against XSS using @SafeHtml validator
4. RESTful/JSON services using Spring MVC 3.2
5. Web App Init using JavaConfig, no web.xml or any xml config (Thank you Josh Long: https://twitter.com/starbuxman & https://github.com/joshlong)
6. Tomcat7 Maven Plugin Config
7. Websocket 1.0 (Coming soon)
8. JMS 2.0/Spring support (Coming soon)

<h2>Setup</h2>

JDK7: http://www.oracle.com/technetwork/java/javaee/downloads/java-ee-sdk-7-downloads-1956236.html <br>
Maven 3: http://maven.apache.org/index.html <br>
Tomcat 7: http://tomcat.apache.org/download-70.cgi <br>
MySQL: http://dev.mysql.com/downloads/mysql/
 
Once JDK7/Maven/Tomcat7/MySQL been installed:

- Modifty tomcat-users.xml, this will give you access to Tomcat manager console, as well as Maven privileges to deploy to tomcat instance:

(tomcatinstalldir)/conf/tomcat_user.xml

    <tomcat-users>
      <role rolename="manager-gui"/>
      <role rolename="manager-script"/>
      <role rolename="manager-jmx"/>
      <role rolename="manager-status"/>
      <user username="admin" password="password" roles="manager-gui,manager-script,managerjmx,manager-status"/>
    </tomcat-users>

- Modify Maven's settings.xml:

.m2/settings.xml or .m2/conf/settings.xml

Add the following under servers element:

    <servers>
      <server>
          <id>localhost</id>
          <username>admin</username>
          <password>password</password>
      </server>
    <servers>

- Fork/Clone the project
- Import project to IDE of choice
- Run the following scripts from mysql cmd or MySQL Workbench

   CREATE SCHEMA `prism` ;


   CREATE TABLE `Person` (
     `id` int(11) NOT NULL AUTO_INCREMENT,
     `uuid` varchar(36) DEFAULT NULL,
     `first_name` varchar(50) NOT NULL,
     `last_name` varchar(50) DEFAULT NULL,
     `email` varchar(100) NOT NULL,
     PRIMARY KEY (`id`),
     UNIQUE KEY `email_UNIQUE` (`email`),
     UNIQUE KEY `uuid_UNIQUE` (`uuid`)
   ) ENGINE=InnoDB AUTO_INCREMENT=20 

   INSERT INTO `prism`.`person`
   (`id`,
   `first_name`,
   `last_name`,
   `email`,
   `uuid`)
   VALUES
   (
   1,
   'john',
   'doe',
   'johndoe@doe.com',
   'a835fe0c-d882-11e2-bbd0-f23c91aec05e'
   );
   
- From command prompt/IDE, run the following commands to deploy the WAR. Ensure your Tomcat7 instance is running:
    - mvn clean install deploy
  If maven fails to deploy try the following:
    - mvn clean install tomcat7:deploy

- Sample RESTful requests using Postman extension in Google Chrome. You can import them directly: 
  - http://www.getpostman.com/collections/f223fe2aa8ce65baa6e6


<h3>Java EE 7: Bean validation using JSR(349) reference implementation Hibernate</h3>

Library: Hibernate Validator 5.0.1.Final

Description:
With JSR 349, one can validate a input form/bean's fields (entire bean graph) and method parameters and return values. This allows a very powerful way to validate state of the object, through the use of annotations. When there is a form field error, we return an error to the client via JSON. Custom Validators can be built, which allows business logic to be validated using annotations - promotes code reuseablity.

Classes:
- com.sampleapp.mvc.model.SignupForm.java
- com.sampleapp.mvc.validator.Account.java (Custom validator example)
- com.sampleapp.mvc.validator.AccountValidator.java (Custom validator example)

Sampel URLs to test Form validation
You could use Postman - REST client Chrome extension to invoke REST calls. To import in Postman:
  - http://www.getpostman.com/collections/f223fe2aa8ce65baa6e6

- POST: http://localhost:8080/sampleapp/prismUser
- Form data: firstName, lastName, email

Reference:
http://beanvalidation.org/1.1/spec/#introduction
http://docs.jboss.org/hibernate/validator/5.0/reference/en-US/pdf/hibernate_validator_reference.pdf (Recommended)

<h3>Spring MVC 3.2 Exception handling using @ControllerAdvice annotation</h3>

Description:
Spring MVC 3.2 introduced @ControllerAdvice annotation. ControllerAdvice annotation brings global handle using @ExceptionHandler, which applies to all classes that are annotated with @Controller annotation. Multiple exceptions can be handled in an exception handler method.

Classes:
- com.sampleapp.mvc.controller.AdviceController.java

Reference:
http://static.springsource.org/spring/docs/3.2.x/spring-framework-reference/html/new-in-3.2.html

<h3>Validate against XSS using @SafeHtml validator</h3>

Description: Ever had to deal with XSS and unsafe HTML getting into the system? Well, with the use of @SafeHTML, we can validate form fields to ensure no evil scripts end up in your db.

Classes:
- com.sampleapp.mvc.model.SignupForm.java

Library:
- Hibernate Validator 5.0.1.Final
- Jsoup 1.7.2

Reference:
http://docs.jboss.org/hibernate/validator/5.0/reference/en-US/html_single/

<h3>RESTful/JSON services using Spring MVC 3.2</h3>

Classes:
- com.sampleapp.mvc.controller.PrismUserController.java

Description: Show how easy it is to to build RESTful services using Spring MVC 3.2 and Jackson JSON mapper api.

<h3>Web Application Init using JavaConfig</h3>

Description: Making use of Servlet 3.0 and Tomcat 7, we can discard web.xml, as well as Spring config files. No more xml files!

Classes:
- com.sampleapp.init.WebAppInitializer.java
- com.sampleapp.db.repository.PersistenceConfiguration.java 

<h3>Tomcat7 Maven Plugin</h3>

Reference: http://tomcat.apache.org/maven-plugin-2.0/tomcat7-maven-plugin/usage.html
