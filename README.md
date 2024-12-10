# Bugreport for ssl location

## Generate Key Bundle
openssl req -x509 -newkey rsa:4096 -keyout myKey.pem -out cert.pem -days 365 -noenc
openssl pkcs12 -export -inkey myKey.pem -in cert.pem -out keyStore.p12 -name certificates

Password: 4af8iU1C0Vrtu3Zhh0sA

Get keystore infos:
openssl pkcs12 -in keyStore.p12 -info -noenc -passin pass:'4af8iU1C0Vrtu3Zhh0sA'


## Before running it
Change location in application.yml to your location:
```
spring:
  ssl:
    bundle:
      jks:
        server:
          keystore:
            location: "D:/repositories/bugreport-ssl-location/keyStore.p12"
```

## Reproduce 'Bug'
Run it with spring-boot-starter-parent 3.3.6:
```
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.3.6</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

Results in:
```
 :: Spring Boot ::                (v3.3.6)

2024-12-10T10:55:17.622+01:00  INFO 38448 --- [bugreport-ssl-location] [           main] c.m.b.BugreportSslLocationApplication    : Starting BugreportSslLocationApplication using Java 21.0.1 with PID 38448 (D:\repositories\bugreport-ssl-location\target\classes started by micha in D:\repositories\bugreport-ssl-location)
2024-12-10T10:55:17.626+01:00  INFO 38448 --- [bugreport-ssl-location] [           main] c.m.b.BugreportSslLocationApplication    : No active profile set, falling back to 1 default profile: "default"
2024-12-10T10:55:18.332+01:00  INFO 38448 --- [bugreport-ssl-location] [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port 443 (https)
2024-12-10T10:55:18.344+01:00  INFO 38448 --- [bugreport-ssl-location] [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2024-12-10T10:55:18.344+01:00  INFO 38448 --- [bugreport-ssl-location] [           main] o.apache.catalina.core.StandardEngine    : Starting Servlet engine: [Apache Tomcat/10.1.33]
2024-12-10T10:55:18.382+01:00  INFO 38448 --- [bugreport-ssl-location] [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2024-12-10T10:55:18.382+01:00  INFO 38448 --- [bugreport-ssl-location] [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 713 ms
2024-12-10T10:55:18.751+01:00  INFO 38448 --- [bugreport-ssl-location] [           main] o.a.t.util.net.NioEndpoint.certificate   : Connector [https-jsse-nio-443], TLS virtual host [_default_], certificate type [UNDEFINED] configured from keystore [C:\Users\micha\.keystore] using alias [certificates] with trust store [null]
2024-12-10T10:55:18.762+01:00  INFO 38448 --- [bugreport-ssl-location] [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port 443 (https) with context path '/'
2024-12-10T10:55:18.767+01:00  INFO 38448 --- [bugreport-ssl-location] [           main] c.m.b.BugreportSslLocationApplication    : Started BugreportSslLocationApplication in 1.487 seconds (process running for 1.991)
```

Now if we use spring-boot-starter-parent from 3.4.0:
```
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.4.0</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

We get:
```
 :: Spring Boot ::                (v3.4.0)

2024-12-10T10:59:23.118+01:00  INFO 17584 --- [bugreport-ssl-location] [           main] c.m.b.BugreportSslLocationApplication    : Starting BugreportSslLocationApplication using Java 21.0.1 with PID 17584 (D:\repositories\bugreport-ssl-location\target\classes started by micha in D:\repositories\bugreport-ssl-location)
2024-12-10T10:59:23.120+01:00  INFO 17584 --- [bugreport-ssl-location] [           main] c.m.b.BugreportSslLocationApplication    : No active profile set, falling back to 1 default profile: "default"
2024-12-10T10:59:23.764+01:00  WARN 17584 --- [bugreport-ssl-location] [           main] ConfigServletWebServerApplicationContext : Exception encountered during context initialization - cancelling refresh attempt: org.springframework.context.ApplicationContextException: Unable to start web server
2024-12-10T10:59:23.768+01:00  INFO 17584 --- [bugreport-ssl-location] [           main] .s.b.a.l.ConditionEvaluationReportLogger : 

Error starting ApplicationContext. To display the condition evaluation report re-run your application with 'debug' enabled.
2024-12-10T10:59:23.783+01:00 ERROR 17584 --- [bugreport-ssl-location] [           main] o.s.boot.SpringApplication               : Application run failed

org.springframework.context.ApplicationContextException: Unable to start web server
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.onRefresh(ServletWebServerApplicationContext.java:165) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:621) ~[spring-context-6.2.0.jar:6.2.0]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:146) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:752) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:439) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:318) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1361) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1350) ~[spring-boot-3.4.0.jar:3.4.0]
	at com.michibaum.bugreport_ssl_location.BugreportSslLocationApplication.main(BugreportSslLocationApplication.java:10) ~[classes/:na]
Caused by: java.lang.IllegalStateException: Could not load store: Unable to create key store: Could not load store from 'D:/repositories/bugreport-ssl-location/keyStore.p12'
	at org.springframework.boot.web.embedded.tomcat.SslConnectorCustomizer.configureSslStores(SslConnectorCustomizer.java:145) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.web.embedded.tomcat.SslConnectorCustomizer.applySslBundle(SslConnectorCustomizer.java:119) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.web.embedded.tomcat.SslConnectorCustomizer.addSslHostConfig(SslConnectorCustomizer.java:95) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.web.embedded.tomcat.SslConnectorCustomizer.configureSsl(SslConnectorCustomizer.java:86) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.web.embedded.tomcat.SslConnectorCustomizer.customize(SslConnectorCustomizer.java:71) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.web.embedded.tomcat.TomcatServletWebServerFactory.customizeSsl(TomcatServletWebServerFactory.java:383) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.web.embedded.tomcat.TomcatServletWebServerFactory.customizeConnector(TomcatServletWebServerFactory.java:359) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.web.embedded.tomcat.TomcatServletWebServerFactory.getWebServer(TomcatServletWebServerFactory.java:212) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.createWebServer(ServletWebServerApplicationContext.java:188) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.onRefresh(ServletWebServerApplicationContext.java:162) ~[spring-boot-3.4.0.jar:3.4.0]
	... 8 common frames omitted
Caused by: java.lang.IllegalStateException: Unable to create key store: Could not load store from 'D:/repositories/bugreport-ssl-location/keyStore.p12'
	at org.springframework.boot.ssl.jks.JksSslStoreBundle.createKeyStore(JksSslStoreBundle.java:112) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.ssl.jks.JksSslStoreBundle.lambda$new$0(JksSslStoreBundle.java:75) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.util.function.SingletonSupplier.get(SingletonSupplier.java:106) ~[spring-core-6.2.0.jar:6.2.0]
	at org.springframework.boot.ssl.jks.JksSslStoreBundle.getKeyStore(JksSslStoreBundle.java:81) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.web.embedded.tomcat.SslConnectorCustomizer.configureSslStores(SslConnectorCustomizer.java:137) ~[spring-boot-3.4.0.jar:3.4.0]
	... 17 common frames omitted
Caused by: java.lang.IllegalStateException: Could not load store from 'D:/repositories/bugreport-ssl-location/keyStore.p12'
	at org.springframework.boot.ssl.jks.JksSslStoreBundle.loadKeyStore(JksSslStoreBundle.java:140) ~[spring-boot-3.4.0.jar:3.4.0]
	at org.springframework.boot.ssl.jks.JksSslStoreBundle.createKeyStore(JksSslStoreBundle.java:107) ~[spring-boot-3.4.0.jar:3.4.0]
	... 21 common frames omitted
Caused by: java.io.FileNotFoundException: class path resource [D:/repositories/bugreport-ssl-location/keyStore.p12] cannot be opened because it does not exist
	at org.springframework.core.io.ClassPathResource.getInputStream(ClassPathResource.java:215) ~[spring-core-6.2.0.jar:6.2.0]
	at org.springframework.boot.ssl.jks.JksSslStoreBundle.loadKeyStore(JksSslStoreBundle.java:135) ~[spring-boot-3.4.0.jar:3.4.0]
	... 22 common frames omitted


Process finished with exit code 1
```

'Bug' is: **Caused by: java.io.FileNotFoundException: class path resource [D:/repositories/bugreport-ssl-location/keyStore.p12] cannot be opened because it does not exist**