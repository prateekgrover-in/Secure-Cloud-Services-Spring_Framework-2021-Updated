### Example 3 : Video Controller with Retrofit

#### Running the Video Service Application

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


#### Understanding the Source Code

The source code can be divided into the main package, and the test package.

1. main package (org.magnum.mobilecloud.video)

The main package contains subpackages for client and controller.

i. client package (org.magnum.mobilecloud.video.client)

Client Package contains one file, VideoSvcApi.java.

a. VideoSvcApi.java

- Interface which contains methods that need to be implemented by the controller.

```java
// 	Retrofit annotations are imported so that clients can automatically convert the interface into a client object
import retrofit.http.Body;
import retrofit.http.GET;
import retrofit.http.POST;

public interface VideoSvcApi {
	
	// 	The path where we expect the VideoSvc to live
	public static final String VIDEO_SVC_PATH = "/video";

	//	Routing the GET Request by client to getVideoList() method of controller.
	@GET(VIDEO_SVC_PATH)
	public List<Video> getVideoList();
	
	//	Routing the POST Request by client to addVideo() method of controller, taking the request body as the Video Object.
	@POST(VIDEO_SVC_PATH)
	public boolean addVideo(@Body Video v);
	
}
```
ii. controller package (org.magnum.mobilecloud.video.controller)

Controller Package contains two files, Video.java and VideoSvc.java

a. Video.java

- Defines a video class containing private variables name, url and Id, and sets up getters and setters for these parameters.

```java
public class Video {

	private String name;
	private String url;
	private long duration;

   	//    An empty constructor is always initialized
	public Video(){}
	
   	//    A constructor with all the parameters.
	public Video(String name, String url, long duration) {
         ... ;
	}
```

- Next, two methods hashCode() and equals() are overriden, with the help of Google's Library (com.google.common.base.Objects) 

```java
   //    Two Videos will generate the same hashcode if they have exactly the same values for their name, url, and duration.
   @Override
	public int hashCode() {
         ...
	}

   //    Two Videos are considered equal if they have exactly the same values for their name, url, and duration.	
   @Override
	public boolean equals(Object obj) {
		   ... ;
	}
```
b. VideoSvc.java

- Implements the Controller, and routs GET and POST Request to its specific methods.
- 
```java
//   Annotation @Controller Tells Spring that this is a controller class.
@Controller

public class VideoSvc implements VideoSvcApi {
	
	//   List that stores the videos that are sent to the server by the client.
	private List<Video> videos = new CopyOnWriteArrayList<Video>();

   	//    Maps all POST Requests to /video path to this method.
	@RequestMapping(value=VIDEO_SVC_PATH, method=RequestMethod.POST)
   
   	//    Converts boolean return type to JSON that can be returned in the body of Response
   	//    Converts JSON Format of body of Request into a Video Object.
	public @ResponseBody boolean addVideo(@RequestBody Video v){
   
      		//    Adds video to list of Videos stored in memory.
		return videos.add(v);
	}
	
   	//    Maps all GET Requests to /video path to this method.
	@RequestMapping(value=VIDEO_SVC_PATH, method=RequestMethod.GET)
   
	//    Converts List<Video> return type to JSON that can be returned in the body of Response
   	public @ResponseBody List<Video> getVideoList(){
		
      		//    Returns list of Videos stored in memory.
      		return videos;
	}

}
```
iii.  Application.java
```java
// Tell Spring that this object represents a Configuration for the application
@Configuration

// Tell Spring to turn on WebMVC (e.g., it should enable the DispatcherServlet so that requests can be routed to our Controllers)
@EnableWebMvc

// Tell Spring to go and scan our controller package (and all sub packages) to find any Controllers or other components that are part of our applciation.
@ComponentScan

// Tell Spring to automatically inject any dependencies that are marked in our classes with @Autowired
@EnableAutoConfiguration

public class Application {

	// Tell Spring to launch the application.
	public static void main(String[] args){
		SpringApplication.run(Application.class, args);
	}
}
```
2. test package (org.magnum.mobilecloud.* .test)

The test package contains two subpackages that test the client and controller respectively.

i. controller test package (org.magnum.mobilecloud.controller.test)

```java
public class VideoSvcTest {
	
	private VideoSvc videoService = new VideoSvc();

	//	This Test simulates a POST Request to add a new Video, and then simulates a GET Request to view the list of videos.
	@Test
	public void testVideoAddAndList() throws Exception {
		// Information about the video
		String title = "Programming Cloud Services for Android Handheld Systems";
		String url = "http://coursera.org/some/video";
		long duration = 60 * 10 * 1000; // 10min in milliseconds
		Video video = new Video(title, url, duration);

		// 	Simulates a POST request to the VideoServlet to add the video.
		boolean ok = videoService.addVideo(video);
		
		assertTrue(ok);
		
		//	SSimulates a GET Request to retrieve all Videos stored in VideoList.
		List<Video> videos = videoService.getVideoList();
		assertTrue(videos.contains(video));
	}
}
```

ii. client test package (org.magnum.mobilecloud.client.test)

``` java
public class VideoSvcClientApiTest {

	//	Defining the URL for Testing the Application
	private final String TEST_URL = "http://localhost:8080";

	//	Using Retrofit library to create an object from the VideoSvcApi interface that can process HTTP Requests
	//	Parameters and Return values are marshalled to/from JSON
	private VideoSvcApi videoService = new RestAdapter.Builder()
			.setEndpoint(TEST_URL)
			.setLogLevel(LogLevel.FULL)
			.build()
			.create(VideoSvcApi.class);


	//	This Test send POST Request from the API Created to add a new Video, and then sends a GET Request to view the list of videos.
	@Test
	public void testVideoAddAndList() throws Exception {
		//	Same function as Controller Test.
	}
}
```

#### What to Pay Attention to

Notice how much has changed from the VideoServlet version of this cloud service:

1. The web.xml file has been eliminated and we just have Application.java now
2. Our VideoSvc has been dramatically simplified compared to the past VideoServlet and now relies on Spring to automatically marshall/unmarshall data sent to/from the client.
3. We have a type-safe interface for interacting with our VideoSvc from a client. This interface, when combined with Retrofit, dramatically simplifies client/server interactions.
4. Look at the two tests in src/test/java. The tests are vastly simpler than with the VideoServlet version. In fact, the Retrofit version of the test (VideoSvcHttpTest) is essentially identical to the version that just constructs and tests a VideoSvc object. Using Retrofit, it looks like you are interacting with a local object -- even though that object is actually sending HTTP requests to the server in response to your method calls.

