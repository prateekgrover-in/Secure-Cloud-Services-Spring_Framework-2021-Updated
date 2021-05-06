This module contains four videos:

1. Spring Dependency Injection & Auto-wiring (https://www.youtube.com/watch?v=IDP-O5VRBjI&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=40)

```java
public interface StorageSystem(){
      public saveToStorage() {}
      public takeBackup() {}
      
      //  Other Storage Methods
}
```
- Let AmazonS3Storage and LocalStorage be classes that implement StorageSystem interface.

```java
@Controller
public class VideoSrc(){
      
      //    Spring automatically filling up the dependancy, Spring will find an object that has the configuration for StorageSystem.
      @Autowired
      private StorageSystem storage;
      
      @Autowired
      private UserManager users;
}
```
- To map the concrete implementations and the specific classes we want to use for themm, we create a configuration.
```java
@Configuration
public class VideoConfiguration(){
      @Bean
      //    @Bean methods will be automatically called at runtime by Spring.
      public StorageSystem StorageSystem(){
            //     We can define the concrete class here, thus decoupling the process.
            return new LocalStorage();
      }
}
```

2. Spring Configuration Annotation (https://www.youtube.com/watch?v=7fYM0bQlBio&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=41)

```java
//    Important for enabling @Autowired annotation
@EnableAutoConfiguration

//    This annotation tells spring to scan the provided package to look for implemented methods (in this case a MediaPlayer), @Autowired annotation.
@ComponentScan({"com/mobile", "com/pc"})

@Configuration
public class VideoConfiguration(){
      
      @Autowired
      private MediaPlayer currPlay;
}
```   

3. Spring Dependency Injection Code Walkthrough (https://www.youtube.com/watch?v=eRq6codSth4&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=42)


