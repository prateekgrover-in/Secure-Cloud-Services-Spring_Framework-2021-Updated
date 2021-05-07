This module contains two videos.

1. Spring Data REST (https://www.youtube.com/watch?v=Udo9j-9v6z8&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=48)

```java
@Controller
public class VideoSrc {
    @Autowired
    private VideoRepo currRepo;
    
    @RequestMapping("/video/save")
    public void saveVideo(Video v){
        currRepo.save(v);
    }
    
    @RequestMapping(" .. ")
    public void deleteVideo(String videoName){
        ...
    }
    
    @RequestMapping(" .. ")
    public List<Video> findByName(String videoName){
        ...
    }
}
```
- These Request Mapping Simple Methods become very repetitive, hence we utilize Spring Data REST, which exposes VideoRepo, and automatically creates REST based methods to interact with Video Repository.

```java
@RepositoryRestResource(collectionResourceRel = "videos", path = "video")
public interface VideoRepo extends CrudRepository <Video, Long> {
      public List<Video> findByName (String toSearchName) {
      }
      public List<Video> findByNameAndCategory (String toSearchName, String toSearchCategory) {
      }
}
//    Spring will scan the "package-name" to find an interface that extends CrudRepository 
@ComponentScan({"package-name")
@EnableJPARepositories
@Import({RepositoryRestMVCConfiguration.class}) 
@Configuration
public class Application{
      
}
```
- Now GET, POST and other requests can be sent directly to the "/video" repository.


2. Spring Data REST Code Walkthrough (https://www.youtube.com/watch?v=4Kr64Bl7jlk&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=49)
