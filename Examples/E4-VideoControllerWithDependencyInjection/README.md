### Example 4 : Video Controller with Dependency Injection

#### Running the Video Service Application

To run the application:

Right-click on the Application class in the org.magnum.mobilecloud.video
package, Run As->Java Application

To stop the application:

Open the Eclipse Debug Perspective (Window->Open Perspective->Debug), right-click on
the application in the "Debug" view (if it isn't open, Window->Show View->Debug) and
select Terminate

#### Accessing the Service

To view a list of the videos that have been added, open your browser to the following
URL:

http://localhost:8080/video

To add a test video, run the VideoSvcClientApiTest by right-clicking on it in 
Eclipse->Run As->JUnit Test (make sure that you run the application first!)

#### Understanding the Source Code

The source code can be divided into the main package, and the test package.

1. main package (org.magnum.mobilecloud.video)

The main package contains 3 sub-packages, client, controller and repository.

i. client sub-package (org.magnum.mobilecloud.video.client)

```java

//    Importing Retrofit query method in addition to last example's imports.
... 
import retrofit.http.Query;

public interface VideoSvcApi {
	
   //    Title parameter of each video, that will be used to search videos.
	public static final String TITLE_PARAMETER = "title";

   ...
	
   //    The path to search videos by title
	public static final String VIDEO_TITLE_SEARCH_PATH = VIDEO_SVC_PATH + "/find";

   ... 
	
   //	Routing the GET Request with TITLE_PARAMETER as query by client to findByTitle() method of controller.
	@GET(VIDEO_TITLE_SEARCH_PATH)
	public Collection<Video> findByTitle(@Query(TITLE_PARAMETER) String title);
   }	
   
```

ii. controller sub-package (org.magnum.mobilecloud.video.controller)

a. Video.java
- Video Class remains the same as last example.

b. VideoSvc.java
- Serves as controller to the server

```java
@Controller
public class VideoSvc implements VideoSvcApi {
	
   //    VideoRepository is a dependency defined by us, that Spring will find an implementation for as we used the @Autowired Annotation
   //    Spring will look for a method with @Bean annotation in the package.
	@Autowired
	private VideoRepository videos;

	...
	
   //    Routs requests with GET requests to /video/find to this method.
	@RequestMapping(value=VideoSvcApi.VIDEO_TITLE_SEARCH_PATH, method=RequestMethod.GET)
   
   //    Converts Return type of Collection of Videos to JSON Response Body
   //    "TITLE_PARAMETER" in the Request Query is used as toSearchTitle parameter in the method
	public @ResponseBody Collection<Video> findByTitle( @RequestParam(TITLE_PARAMETER) String toSearchTitle) {
		
         //    Returns List of videos that contain toSearchTitle in their title.
         return videos.findByTitle(toSearchTitle);
	}

}
```

c. Application.java

```java

//    Similar annotations as of last example.
...

public class Application {

   ...
   
   //    Inject the Object of NoDuplicatesVideoRepository Class to the Dependency of Controller which has @Autowired annotation.
	@Bean
	public VideoRepository videoRepository(){
		   return new NoDuplicatesVideoRepository();
	}
	
}
```

iii.  repository subpackage (org.magnum.mobilecloud.video.repository)









## What to Pay Attention to

In this version of the VideoSvc application, we have added dependency injection:

1. The "videos" member variable of VideoSvc is not explicitly initialized in the VideoSvc
   class. Instead, this member variable is marked with @Autowired. Spring automatically
   connects the value for this member variable to whatever type of VideoRepository we
   construct in our @Bean annotated Application.videoRepository() method. As long as
   your Application class has exactly one @Bean annotated method that returns an instance
   of a particular interface, Spring can automatically find every occurrence where you have
   asked for that interface to be @Autowired and inject the value you return from your
   method into those objects. 
2. This version updates the VideoSvcTest to show how mock objects can be injected into
   @Autowired member variables. In this case, a test mock VideoRepository (constructed
   using the Mockito framework) is injected into the "videos" member variable of VideoSvc.
3. The tests add a new integration test that fully "wires" your controller using dependency
   injection and sends mock HTTP requests to it. This test helps ensure that your Application
   is properly configuring all of the @Autowired values that are needed in your application
   and that they all work together correctly.
