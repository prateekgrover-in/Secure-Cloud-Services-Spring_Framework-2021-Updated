## Example 5 - Video Controller with Java Persistence API

## Running the Video Service Application

To run the application:

Right-click on the Application class in the org.magnum.mobilecloud.video
package, Run As->Java Application

To stop the application:

Open the Eclipse Debug Perspective (Window->Open Perspective->Debug), right-click on
the application in the "Debug" view (if it isn't open, Window->Show View->Debug) and
select Terminate

## Accessing the Service

To view a list of the videos that have been added, open your browser to the following
URL:

http://localhost:8080/video

To add a test video, run the VideoSvcClientApiTest by right-clicking on it in 
Eclipse->Run As->JUnit Test (make sure that you run the application first!)

## Understanding the Source Code

The source code contains two packages, main and test.

## main package (org.magnum.mobilecloud.video)

The main package contains three subpackages, client, controller and repository.

### i. video sub-package (org.magnum.mobilecloud.video.repository)
- video sub-package contains Video.java and VideoRepository.java as its two Classes.

a. Video.java

```java
//    To define type of objects in database, @Entity annotation of JPA is used.
@Entity
public class Video {

   //    To define auto-generated values of an object in database, @Id annotation of JPA is used.
	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private long id;

	private String name;
	private String url;
	private long duration;
   
   ... 
}
```

b. VideoRepository.java
- Using Spring Annotations to automate the boiler-plate code that was previously written.
```java
//    Using Spring Annotations to define a CRUD Repository
@Repository
public interface VideoRepository extends CrudRepository<Video, Long>{

	// Find all videos with a matching title (e.g., Video.name)
	public Collection<Video> findByName(String title);
	
}
```

### ii. client sub-package (org.magnum.mobilecloud.video.client)

- Client sub-package contains VideoSvcApi interface, same as the last example with Retrofit annotations, acting as the connection between client and server.

```java
public interface VideoSvcApi {
	
	public static final String TITLE_PARAMETER = "title";

	// The path where we expect the VideoSvc to live
	public static final String VIDEO_SVC_PATH = "/video";

	// The path to search videos by title
	public static final String VIDEO_TITLE_SEARCH_PATH = VIDEO_SVC_PATH + "/find";

	@GET(VIDEO_SVC_PATH)
	public Collection<Video> getVideoList();
	
	@POST(VIDEO_SVC_PATH)
	public boolean addVideo(@Body Video v);
	
	@GET(VIDEO_TITLE_SEARCH_PATH)
	public Collection<Video> findByName(@Query(TITLE_PARAMETER) String title);
	
}
```

### iii. controller sub-package (org.magnum.mobilecloud.video.controller)
- To interact with databases, several methods need to be altered.
```java
@Controller
public class VideoSvc implements VideoSvcApi {
	
   //    @Autowired to find implementations of VideoRepository by Spring
	@Autowired
	private VideoRepository videos;

   //    save() method is used to save the content of request converted to Video Object to database
	@RequestMapping(value=VideoSvcApi.VIDEO_SVC_PATH, method=RequestMethod.POST)
	public @ResponseBody boolean addVideo(@RequestBody Video v){
		 videos.save(v);
		 return true;
	}
	
	//    findAll() method is used to list all the Video objects stored in the database.
	@RequestMapping(value=VideoSvcApi.VIDEO_SVC_PATH, method=RequestMethod.GET)
	public @ResponseBody Collection<Video> getVideoList(){
		return Lists.newArrayList(videos.findAll());
	}
	
   //    finds videos with toSearchTitle stored in the database.
   @RequestMapping(value=VideoSvcApi.VIDEO_TITLE_SEARCH_PATH, method=RequestMethod.GET)
	public @ResponseBody Collection<Video> findByName( @RequestParam(TITLE_PARAMETER) String toSearchTitle) {	
         	//    Returns List of videos that contain toSearchTitle in their title.
         	return videos.findByName(toSearchTitle);
	}

}
```
### iv. Application.java
- Enable JPA Repositories in main driver class of the server.

```java
//    Tell Spring to automatically inject any dependencies that are marked in our classes with @Autowired
@EnableAutoConfiguration

// Tell Spring to automatically create a JPA implementation of our VideoRepository
@EnableJpaRepositories(basePackageClasses = VideoRepository.class)

// Tell Spring that this object represents a Configuration for the application
@Configuration

// Tell Spring to turn on WebMVC (e.g., it should enable the DispatcherServlet so that requests can be routed to our Controllers
@EnableWebMvc

// Tell Spring to go and scan our controller package (and all sub packages) to find any Controllers or other components that are part of our applciation.
@ComponentScan

public class Application {
	
	// Tell Spring to launch our Application
	public static void main(String[] args) {
   
		SpringApplication.run(Application.class, args);
	}
	
}
```
## test package

- test package contains three sub-packages for testing the controller, integration and video.

### i. video test sub-package (org.magnum.mobilecloud.video)
- Creates a random video, and passes it in JSON Format when method called, same as last example.
```java
public class TestData {

	// 	Jackson ObjectMapper which will help in converting from Object to JSON
	private static final ObjectMapper objectMapper = new ObjectMapper();
	
	// 	Constructs a random video using UUID Class.
	public static Video randomVideo() {
		... 
	}
	
	//	Convert an object to JSON using Jackson's ObjectMapper
	public static String toJson(Object o) throws Exception{
		... 
	}
}
```
### ii. integration test sub-package (org.magnum.mobilecloud.integration.test)
- This sub-package contains two Java Classes: 
a. VideoSvcClientApiTest.java 
```java
public class VideoSvcClientApiTest {

	//	Defining the URL for Testing the Application
	//	Using Retrofit library to create an object from the VideoSvcApi interface that can process HTTP Requests
	
	// 	Initializes a random video from video sub-package
	private Video video = TestData.randomVideo();
	
	//	This Test send POST Request from the API Created to add a new Video, and then sends a GET Request to view the list of videos.
	@Test
	public void testVideoAddAndList() throws Exception {
		//	Same function as Controller Test.
	}
}
```

b. VideoSvcIntegrationTest.java
```java
.. Annotations as last example.

public class VideoSvcIntegrationTest {

	// 	Ask Spring to automatically construct and inject your VideoSvc into the test
	@Autowired
	private VideoSvc videoService;

	// 	This is the mock interface to our application that we will use to send mock HTTP requests
	private MockMvc mockMvc;

	// 	Executed before each test
	@Before
	public void setUp() {
		// Setup Spring test in standalone mode with our VideoSvc object that it built
	}
	
	//	Similar to last example, we will try to simulate GET and POST requests here using Mockito
	@Test
	public void testVideoAddAndList() throws Exception {
	
		//	To test our servers, we generate a Random Video from video test sub-package, and its JSON.
		...
			
		// 	Send a request that should invoke VideoSvc.addVideo(Video v) and check that the request succeeded
		...
		
		// 	Send a request that should invoke VideoSvc.getVideos() and check that the Video object added above is in list of returned videos.
		...
	}
}
```
### iii. controller test sub-package (org.magnum.mobilecloud.controller.test)
```java
// 	Test uses Mockito Library to inject VideoRepository through dependency injection into VideoSvc
public class VideoSvcTest {

	// 	@Mock Annotation tells Mockito to create a mock object for VideoRepository.
	//	Mock oobjects are 'fake' implementations, that can be scripted to provide specific outputs in response to specific inputs.
	@Mock
	private VideoRepository videoRepository;

	// 	Automatically inject the mock VideoRepository into the VideoSvc object
	@InjectMocks
	private VideoSvc videoService;

	// 	Takes random video, from video test sub-package
	private Video video = TestData.randomVideo();

	//	Code inside Before is implemented before each test case.
	@Before
	public void setUp() {
		// 	We do not need to define specific methods for getVideos and addVideo as JPA exposes our repository of videos and handles
		// 	basic boiler-plate operations on its own.
	
		// 	Process mock annotations and inject the mock VideoRepository into the VideoSvc object
		MockitoAnnotations.initMocks(this);

		// 	Tell the mock VideoRepository to always return the random Video object that we create above when its getVideos() method is called
		when(videoRepository.findAll()).thenReturn(Arrays.asList(video));
	}
	
	//	This Test simulates a POST Request to add a new Video, and then simulates a GET Request to view the list of videos.
	@Test
	public void testVideoAddAndList() throws Exception {
		...
		//	Same as last example.
	}
```
## What to Pay Attention to

In this version of the VideoSvc application, we have added dependency injection:

1. The VideoRepository interface defines the application's interface to the database. There is no implementation of the VideoRepository in the project, Spring dynamically creates the implementation when it discovers the @Repository annotated interface.
2. This "videos" member variable of the VideoSvc is automatically auto-wired with the implementation of the VideoRepository that Spring creates. 
3. The VideoRepository inherits methods, such as save(...), that are defined in the CrudRepository interface that it extends. 
4. The "compile("com.h2database:h2")" line in the build.gradle file adds the H2 database as a dependency and Spring Boot automatically discovers it and embeds a database    instance in the application. By default, the database is configured to be in-memory only and will not persist data across restarts. However, another database could  easily be swapped in and data would be persisted durably.
5. Notice that the VideoRepository is automatically discovered by Spring.
