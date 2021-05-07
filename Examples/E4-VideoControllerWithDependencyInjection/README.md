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

This subpackage contains a VideoRepository interface and its two separate implementations.

a. VideoRepository.java
```java
public interface VideoRepository {

	// Add a video
	public boolean addVideo(Video v);
	
	// Get the videos that have been added so far
	public Collection<Video> getVideos();
	
	// Find all videos with a matching title (e.g., Video.name)
	public Collection<Video> findByTitle(String title);
	
}
```
- The two implementations allow and do not allow Duplicates in their Repositories respectively.

b. NoDuplicatesVideoRepository.java
```java
public class NoDuplicatesVideoRepository implements VideoRepository {

	// Set will only store one instance of each duplicate, (checks duplication via .equals() function)
	private Set<Video> videoSet = Collections.newSetFromMap(new ConcurrentHashMap<Video, Boolean>());
	
	@Override
	public boolean addVideo(Video v) {
		return videoSet.add(v);
	}

	@Override
	public Collection<Video> getVideos() {
		return videoSet;
	}

	// Search the list of videos for ones with matching titles.
	@Override
	public Collection<Video> findByTitle(String toSearchTitle) {
		Set<Video> matches = new HashSet<>();
		for(Video video : videoSet){
			if(video.getName().equals(toSearchTitle)){
				matches.add(video);
			}
		}
		return matches;
	}

}
```

c. AllowsDuplicatesVideoRepository.java
```java
public class AllowsDuplicatesVideoRepository implements VideoRepository {

	// Lists allow duplicate objects that are .equals() to each other
	private List<Video> videoList = new CopyOnWriteArrayList<Video>();
	
	...
	
```

2. test package (org.magnum.mobilecloud.* .test)

test package contains three sub-packages for testing the controller, integration and video.

i. controller test package  (org.magnum.mobilecloud.controller.test)
```java
/**
 * 
 * This test directly invokes the methods on VideoSvc to test them. The test
 * uses the Mockito library to inject a mock VideoRepository through dependency
 * injection into the VideoSvc object.
 * 
 * To run this test, right-click on it in Eclipse and select
 * "Run As"->"JUnit Test"
 * 
 * Pay attention to how this test that actually uses HTTP and the test that just
 * directly makes method calls on a VideoSvc object are essentially identical.
 * All that changes is the setup of the videoService variable. Yes, this could
 * be refactored to eliminate code duplication...but the goal was to show how
 * much Retrofit simplifies interaction with our service!
 * 
 * @author jules
 *
 */
public class VideoSvcTest {

	// This tells Mockito to create a mock object for the VideoRepository
	// implementation that will be used for this test. A mock object is a
	// "fake" implementation of the interface that we can script to provide
	// specific outputs in response to different inputs.
	@Mock
	private VideoRepository videoRepository;

	// Automatically inject the mock VideoRepository into the VideoSvc
	// object
	@InjectMocks
	private VideoSvc videoService;

	private Video video = TestData.randomVideo();

	@Before
	public void setUp() {
		// Process mock annotations and inject the mock VideoRepository
		// into the VideoSvc object
		MockitoAnnotations.initMocks(this);

		// Tell the mock VideoRepository implementation to always return
		// true when its addVideo() method is called
		when(videoRepository.addVideo(any(Video.class))).thenReturn(true);
		
		// Tell the mock VideoRepository to always return the random Video
		// object that we create above when its getVideos() method is called
		when(videoRepository.getVideos()).thenReturn(Arrays.asList(video));
	}
	
	
	// Yes, this test doesn't do much because VideoSvc is
	// essentially delegating to VideoRepository. The goal is to
	// provide a simple example of testing controllers with mock
	// objects and dependency injection.
	@Test
	public void testVideoAddAndList() throws Exception {

		// Ensure that calling addVideo works
		boolean ok = videoService.addVideo(video);
		assertTrue(ok);

		// Make sure that the Video we added is in the list
		Collection<Video> videos = videoService.getVideoList();
		assertTrue(videos.contains(video));
	}

}
```

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
