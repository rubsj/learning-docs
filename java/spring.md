### Spring boot

## components and beans
- To define a component in Spring, annotate a class with any of the following annotations:
    • @Component
    • @Service
    • @Repository
    • @Controller/@RestController
- Spring will automatically create an instance - a "bean". The bean will have a name (by default it's the same as the class name, with first letter lowercased).
- You can specify a name for the component, like this: `@Component("myCustomName")`
- By default, Spring creates a singleton bean instance. Possible scopes are `prototype` , `request` , `session` , `application`
- Spring always eagerly creates singleton beans i.e. when application starts. This can slow down the startup time.
- You can tell spring to lazily-instantiate a bean using @Lazy annotation
    - this avoids creating bean at starup 
    - bean gets created when its needed for the first time
- In SpringBoot , you can access a bean in your program via the ApplicationContext object.
    ` ApplicationContext context = SpringApplication.run(LearnSpringApplication.class, args);`
- When SpringBoot app starts, it scans for component classes , it looks into application class package plus subpackages
    - you can tell spring to look elsewhere using `scanBasePackages` param of `@SpringBootApplication` annotation 
      i.e. `@SpringBootapplication(scanBasePackages={"pkg1", "pkg2"})`

### Autowiring
      


