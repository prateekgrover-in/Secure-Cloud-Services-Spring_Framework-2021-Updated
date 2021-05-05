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
    @RequestMapping("\contacts")
    
    public Contacts getContacts(){
        //    Retrieve the contacts
        Contacts c =  ... ;
        return c;
        
    @RequestMapping("\contacts")    
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
  
      @RequestMapping("\search")
      
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
      @RequestMapping("\search\{str}")
      
      //   the path Variable can be now assigned to searchStr argument of the method.
      public Contacts search ( @PathVar("str") String searchStr, @RequestParam("flag") int searchFlag){
            //   Implementation of Search Function
      }
}
```
