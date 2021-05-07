This module contains ten videos:

1. Sessions (https://www.youtube.com/watch?v=4hf5CpICW2s&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=52)

- Sessions are about maintaining a dialogue between the client and the server even in the absence of connection between them. Remembering the user's actions, and methods it had implemented. How long do we want to maintain a session? Depends on Resources and Safety.
- What information should be maintained during a session? 
- Set up a authentication service, to maintain the safety of a session, so that an attacker cannot intercept the data in sessions.
- Many libraries have been developed for this process, with continuous testing.
- Cookies are used to keep session information with the server. We can have purely server side sessions storing all data on server, or partial data on server storing only the sensitive information. Cookies on client side can help server identify the client and maintain the session and state. Usually cryptographic information on Session ID is sent to the client, to keep unique state with each client.

2. Understanding Session Security (https://www.youtube.com/watch?v=SryTD-brRnw&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=53)

- IP addresses or Mac Addresses could be used, but this data can be spoofed and thus vulnerable to attacks. So, most applications send session cookies to the client. The client would enter username and password over HTTPS, the server then checks the validity of their information, and then sends a session cookie to the client, that will help in remembering the client in future. Response from the Sever should also be on HTTPS, so that no attacker can impersonate another user.
- Now the client will send the session cookie with each subsequent request, and thus facilitate the connection.

3. Spring Security Overview (https://www.youtube.com/watch?v=rWbOaBC9Noc&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=54)

- In Spring Framework, a client interacts with dispatcher servlet which routs the requests to specific controllers. To establish a session with dispatcher servlet, Spring Security framework can be used to provide session cookies back to the client while processing its request.
- Spring Security contains a filter which can detect protected requests from a client which does not have session cookies, it blocks the requests, as it has not been logged in, and forces them to log in. Eg: uploading a video on one's channel without logging in. Acts as a gatekeeper to the resource or a protected method on a controller. Thus we can specify protected methods using Spring Security.

4. Building a Custom UserDetailsService (https://www.youtube.com/watch?v=KmCvyg20xi4&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=55)

- UserDetailsService is an interface that allows Spring to look up a user by name. If a client tries to access uploadVideos without any cookies, Spring Security will force the user to log in first, after which it will pass the login details to UserDetailService, which then looks up the name in the allowed accounts, which would contain the passwords. This can be anything other than form-based login also. 
- UserDetailService returns the name, password, validity of account, list of granted authorities ie. the role and the methods it is allowed to access.

7. Spring Security Method Annotations (https://www.youtube.com/watch?v=6qxywFQ6IhE&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=56)

- Let Bob be allowed access only to upload videos, Alice be allowed to delete videos, and Meghan be allowed to upload and delete videos.
```java
@Controller
public class VideoSvc {

      @Preauthorize("hasRole(user)")
      public void uploadVideo () {
            ... ;
      }
      
      @Preauthorize("hasRole(admin)")
      public void deleteVideo (String toDelete){
            ... ;
      }
}
```
- So, now Bob can have the role "user", Alice : "user", Meghan : "user", "admin". So all of these users will have to login first, then their privilege grants are checked after which method access is provided.

8. Accessing User Accounts When Handling Requests (https://www.youtube.com/watch?v=BPCSrUNJxg0&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=57)
```java
@Controller
public class VideoSvc {

      @Preauthorize("hasRole(user)")
      //    Principal p is the current user that is trying to access the method
      
      public boolean uploadVideo ( .. , Principal p) {
            Video v = new Video();
            ... ;
            String user = p.name;
            v.setUser(user);
      }

}
```
- another way to achieve this would be: 
```java
public class Video  {
      private String owner;
      public String getOwner() {
            ...
      }
      ...
}
```

```java
@Controller
public class VideoSvc {

      @Preauthorize("#v.owner == principal.name")
      //    This would tell Spring, to check the owner of the video to match client's name, in addition to checking the validity of the request.
      
      public boolean uploadVideo (Video v, Principal p) {
            ...
            String user = p.name;
            v.setUser(user);
            ...
      }
}
```
