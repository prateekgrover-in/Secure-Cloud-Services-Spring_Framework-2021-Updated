### Better Client-side Communication Abstractions

This module contains 3 videos:

1: Introduction to Retrofit (https://www.youtube.com/watch?v=yUuJJf1OFl8&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=38)

- Open-Source Library by Square, for client to interact.

```java
public interface VideoSrc(){
      @GET("/video")
      public String getVideos(){}
      
      @FormURLEncoded
      @POST("/video")
      public void addVideo( @Field("name") String name, @Field("url") String url, @Field("duration") long duration){}
}
```

- RetroFit library can create an Object from the above Interface and process the HTTP Request, by the below procedure.

```java
RestAdapter adapt = new RestAdapter.setEndPoint("http://localhost").build();
VideoSrc src = adapt.create(VideoSrc.class);
String videos = src.getVideos();
src.addVideo("Coursera", "http://youtube.com", 2600);
```

- RetroFit library goes from Java to HTTP to Java for getVideos() method, and again to HTTP and to Java for addVideo() method.

2. Retrofit Client Code Walkthrough (https://www.youtube.com/watch?v=q0S1QWL9Z5M&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=39)

Walks through Client Code for example/VideoControllerAndRetrofit


3. Android Retrofit Client Code Walkthrough (Not available anymore).
