pf4j-spring-boot introduce plugin oriented programming to Spring Boot. It is inspired and builds 
on top of [Pf4j](https://pf4j.org/) project. 

### Why we need plugin for Spring Boot?
* Spring boot is good, but it is monolithic. It means you have to delivery the whole application
for every single code change every time.
* We need a modern framework with flexibility and extensibility to rapidly deliver solution 
for complex business scenario. 
* Not all projects need to consider scaling at the beginning stage, like Spring Cloud does.
* With pf4j-spring-boot, we could think in micro service architecture only with Spring Boot,
but no need to worried too much about "cloud native" things, like service discovery, traffic control, etc.
* It's a medium level preference between monolithic Spring Boot application and distributed 
Spring Cloud application.

### Feature / Benefits
* Turn monolithic Spring Boot application to be **MODULAR**.
* Much more **LIGHTWEIGHT** development and deployment for spiral extensible architecture.
* Install/update/start/stop plugins **ON THE FLY**.
* **FULL STACK** Web/Rest server-side features powered by Spring Boot, including:
    * controller for request handling
    * persistence (Spring Data/JPA/Jooq/Plain SQL)
    * security
    * AOP
    * resource loading
* Code and test plugin project as **STANDALONE** Spring Boot project.
* **NO** extra knowledge **NEED TO LEARN** as long as you are familiar with Spring Boot.
* **NO XML**

##### vs OSGi Based Server ([Eclipse Virgo](https://www.eclipse.org/virgo/) \ [Apache Karaf](http://karaf.apache.org/))
* OSGi based server need a server container to deploy, which is not _cloud friendly_.
* OSGi configuration is complex and strict, the learning curve is steep.
* Bundle cannot be run standalone, which is hard for debug and test.
* ~~Spring dm server is dead, and its successor Virgo is now still struggle to live.~~ 

##### vs Spring Cloud
* Spring Cloud does not aim to solve similar problem as pf4j-spring-boot, but so far it maybe the 
best choice to solve application extension problem.
* With pf4j-spring-boot, you will no need to consider too much about computer resource arrangement, 
everything stick tight within one process.
* Again, pf4j-spring-boot is good for medium level applications, which have problem to handle business
change rapidly. For large scale application, Spring Cloud is still your best choice.
* pf4j-spring-boot should be able to live with Spring Cloud. After all, it is still a Spring Boot
application, just like any single service provider node in Spring Cloud network.

### Getting Start

##### Create app project
1. Create a Spring Boot project with multi sub project structure.
    1. For Gradle, it means [multiple projects](https://docs.gradle.org/current/userguide/intro_multi_project_builds.html)
    2. For Maven, it means [multiple modules](https://maven.apache.org/guides/mini/guide-multiple-modules.html)
    3. Take the demo projects for reference.
2. Introduce `pf4j-spring-boot-starter` to dependencies.
    * Maven
        ```
        TBD
        ```
    * Gradle
        ```
        TBD
        ```
4. Add belows to `application.properties`.
    ```
    spring.pf4j.runtimeMode = development
    spring.pf4j.enabled = true
    # remember to add this line in case you are using IDEA
    spring.pf4j.classes-directories = "out/production/classes, out/production/resources"
    ``` 
4. Add anything you want in this project like `Controller`, `Service`, `Repository`, `Model`, etc.
3. Create an empty folder named `plugins`.

##### Create plugin project
1. Create a plain Spring Boot project in the `plugins` folder. 
2. Add `plugin.properties` file to the plugin project.
    ```properties
    plugin.id=<>
    plugin.class=demo.pf4j.admin.AdminPlugin
    plugin.version=0.0.1
    plugin.provider=Hank CP
    plugin.dependencies=
    ```
3. Add Plugin class
    ```java
    public class DemoPlugin extends SpringBootPlugin {
    
        public DemoPlugin(PluginWrapper wrapper) {
            super(wrapper);
        }
    
        @Override
        protected SpringBootstrap createSpringBootstrap() {
            return new SpringBootstrap(this, AdminPluginStarter.class);
        }
    }
    ```
4. Add anything you want in the plugin project like `Controller`, `Service`, `Repository`, `Model`, etc.
 
Everything is done and now you could start the app project to test the plugin. 

##### Checkout the demo projects for more details
* demo-shared: Shared code for app and plugin projects.
* demo-security: Security configuration demonstrate how to introduce Spring Security and secure your 
API via AOP.
* demo-apis: Class shared between app\<-\>plugins and plugins\<-\>plugins need to be declared as API.
* demo-app: The entry point and master project. It provides two `SpringApplication`
    * `DemoApp` does not have Spring Security configured.
    * `DomeSecureApp` include Spring Security, so you need to provide authentication information
    when access its rest API.
* plugins
    * demo-plugin-admin
        * Demonstrate Spring Security integration to plugin projects
        * Demonstrate [Spring Boot profile](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-profiles.html) 
        feature support.
    * demo-plugin-author: Demonstrate share resource (like DataSource, TransactionManager) between
    api/plugins.
    * demo-plugin-library: Demonstrate using Spring Data/JPA.
    * demo-plugin-shelf: Demonstrate api expose and invocation between plugins.
* Every single projects with `SpringApplication` could be run standalone.
* It is basically a skeleton project that you could starts your own project. It almost contains 
everything we need in the real project. 

### Documentation
* [Plugin driven design]()
* [How it works]()
    * Plugin loading by pf4j
    * Get involved with Spring `ApplicationContext` initialization process.
    * Constraint
* [Configuration]()
* [Request Mapping]()
* [Persistence]()
    * Plain SQL / Jooq
    * Spring Data (JPA / Hibernate)
    * NoSQL (Mongo / Redis)
* [Security / AOP]()
* [Deployment]()
* [Weaknesses]()
* [Trouble Shoot]()
* [TODO]()

### Credit & Contribution

### TODO
* Cache (n+1 problem)
* Distributed Transaction Support
* More elegant way to register plugin controller
* Actuator support
* Management console
* Front End modularization
* Continuous Deployment

### Note
* Package name cannot start with `org.pf4j`
* Stay in same framework stack, don't introduce library with complex dependencies into plugin, e.g Spring data JPA

### License 

```
/*
 * Copyright (C) 2019-present the original author or authors.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */
```