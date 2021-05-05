This module contains 13 videos.

1. Intro to Java Annotations (https://www.youtube.com/watch?v=lA1n_HBrFwY&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=25)

4. The Spring Dispatcher Servlet and Controller Abstraction (https://www.youtube.com/watch?v=zhWwEAZQ-FI&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=28)

- In normal Java Servlets the process flow to handle request is:
    doGet(Request){
        i. Extract the param from Request URL.
        ii. Validate the param. (Not null, not invalid)
        iii. construct Java Objects using these request param.
        iv. Logic to handle request.
    }
- In any Web Request in Java Servlet, I need to follow steps i,ii,iii. Which is unnecessary operations to follow each time. These code is called Boiler Plate code.
- One of the solutions to remove boiler plate code is use of Spring framework. Spring Framework has specialized servlets, "DispatcherServlets" whose operation is similar to Java Servlets.
- DispatcherServlets has "Controller" which handles the request. DispatcherServlets remove the boiler plate code.
- Another layer of Routing can be represented in "Controller", to assign a request to a particular method within a servlet.
- It will look at the parameters of the method, and try to extract the datatypes of these parameters from the HTTP Request, by matching the type of parameter.
- Can also take in a series of requests and "marshal" them into parameters of the methods.

5. Intro to Spring Controllers (https://www.youtube.com/watch?v=ltNV48OsDHA&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=29)
- 
```java
//   Annotation @Controller Tells Spring that this is a controller class.
@Controller

public class ContactsCtrl(){
    
    //    @RequestMapping assigns requests with given path to given method.
    @RequestMapping("/contacts")
    
    public Contacts getContacts(){
        //    Retrieve the contacts
        Contacts c =  ... ;
        return c;
        
    @RequestMapping("/contacts")    
    public Contacts friends(){
        //     Retrieve friends
        Contacts f = ... ;
        return f;
    }
}
```

6. Accepting Client Data with RequestParam Annotations (https://www.youtube.com/watch?v=SEAfI4CE6Os&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=30)

```java
@Controller
public class ContactsCtrl(){
  
      @RequestMapping("/search")
      
      //    Dispatcher Servlet will extract parameters "string" and "flag" from String datatype to required datatype checking for NULL, and invalid values automatically.
      public Contacts search ( @RequestParam("search") String searchStr, @RequestParam("flag") int searchFlag){
            //   Implementation of Search Function
      }
}
```

7. Accepting Client Data with PathVar Annotations (https://www.youtube.com/watch?v=O2QC9Ym40Lk&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=31)

- Getting Data that might be present in RequestPath rather than Request Parameters, and passing into methods.

```java
@Controller
public class ContactsCtrl(){
  
      //   {str} is a path Variable, which would assign any path with given format of URL to this method
      @RequestMapping("/search/{str}")
      
      //   the path Variable can be now assigned to searchStr argument of the method.
      public Contacts search ( @PathVar("str") String searchStr, @RequestParam("flag") int searchFlag){
            //   Implementation of Search Function
      }
}
```

8. Accepting Client Data with Request Body Annotations & JSON (https://www.youtube.com/watch?v=gSTI3SDuGuo&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=32)

- Defining a Search Object 
```java
public class Search {
    private String firstName;
    private String lastName;
    
    //  Getters and Setters

```
- Converting Request Data into Objects, HTTP Message Converters constructs an object from the data passed by the client into Java Objects automatically.
```java
@Controller
public class ContactsCtrl(){
 
      @RequestMapping("/search/")
      public Contacts search (@RequestBody Search s){
            //   Implementation of search Function
      }
}
```

9. Handling Multipart Data (https://www.youtube.com/watch?v=QxCDtA0vsrY&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=33)
- To pass large objects like data for videos
```java
@Controller
public class VideoSrc(){
    public boolean uploadVideo(@RequestParam("data") MultipartFile videoData){
        InputStream = videoData.getInputStream();
        //   Continue the Procedure
    }
}
```
- When we use Multipart Files, we need to add a method to the Application Class, specifying the boundaries of parameters.
```java
public class Application(){
    @Bean
    public MultipartConfigElement getMultipartConfig(){
        MultipartConfigFactory f = ... ;
        f.setMaxFileSize(2000)
        f.setMaxRequestSize( ... );
        f.createMultipartConfig();
    }
}
```

10. Generating Responses with the ResponseBody Annotation (https://www.youtube.com/watch?v=uhtb2nZDpxc&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=34)
```java
@Controller
public class ContactsCtrl(){
 
      @RequestMapping("/search/")
      
      //    @RequestBody helps in conversion of Contact Object into JSON Object, and return it in the ResponseBody.
      public @ResponseBody Contacts search ( ... ){
            //   Implementation of search Function
      }
}
```
- Now a complete path has been developed from Request to Response. This is a huge amount of work, but Spring does most of the heavy lifting, we only need annotations to tell what work it needs to do.

11. Custom Marshalling with Jackson Annotations (https://www.youtube.com/watch?v=jkX4HZIQFmU&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=35)

- Jackson - Convert JSON Payload into Java Object vise versa:
- In RESTFul API, JSON Payload is mapped into Java Object. This can be achieved by Jackson library.
For example : 
```java
public class Video {
        private String name;
        private String URL;
        private long duration;

        public Video(String name, String URL, long duration) {
            this.name = name;
            this.URL = URL;
            this.duration = duration;
        }
        
        // Getters and Setters
}
```
- Let ResponseBody containing JSON payload with name, URL and duration.
```json
    "{
        "name" : "jacksonVideo",
        "URL" : "http://localhost:8080/getJackson",
        "duration" : 12345
    }"
```
- Jackson Library API receives JSON payload and converts / maps into object of Video class. 
```java
    //    Inner Workings
    public Video getJacksonVideo(@RequestBody Video v) {
        Video newVideo = new Video(v.getName(), v.getURL(), v.getDuration());
        return newVideo;
    }
```
- Jackson takes the JSON payload and mapped each attribute with appropriate Setter method.
For eg:
        Jackson takes "name" param and mapped the value with "setName()" method of Video class.
        Similarly, all the param mapped with their appropriate SetterMethod.

- If we want Jackson to ignore some parameters and not convert them into Object then use @JsonIgnore.
```java
        @JsonIgnore
        public void setDuration(long duration){
            this.duration = duration
        }
```
- Here, SetDuration method is annotated as @JsonIgnore which tells Jackson lib to not convert JSON param into class object.

12. Spring Boot & Application Structure (https://www.youtube.com/watch?v=yXhFn8uWqLI&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=36)

- Currently we will need to setup Web Container, web.xml, DispatcherServlet and so on, a lot of work that involves boiler-plate code.
- Spring Boot: a sub-project of Spring Framework automates all of the setup described, and lets us focus on writing Controllers.
- We can specify configurations to be as intensive as we want, or as default as we want.
- So, we have a set of Controllers, where all of the Logic resides, we have an Application Class which contains Configurations for Controllers, in the main method of Application Class, we can apply SpringApplication.run(), to start up the application.
- It sets up the embedded web containers, discover the controllers, setup the DispatcherServlet and connecting to databases and so on.

13: Spring Controller Code Walkthrough (https://www.youtube.com/watch?v=KSo1TtQtco0&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=37)

- Code Walkthrough for example/VideoContollerAndRetroFit
