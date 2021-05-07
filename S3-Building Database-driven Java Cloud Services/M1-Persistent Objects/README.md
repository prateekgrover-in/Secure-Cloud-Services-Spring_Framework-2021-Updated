This module contains 6 videos:

1. Object to Database Mapping (https://www.youtube.com/watch?v=NGHM52ZDR_8&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=43)

- Client works with Objects of the class which marshalled into JSON to use in HTTP Body (Client Side operation) using Jackson Framework.
- At Server Side, JSON objects converted into objects that can be saved to Database Tables, we could match each object to a row of the database, and columns can be data attributes.
- This process is called Object Relational Mapping with Relational Databases & maybe with even Non-Relational Databases

2. The Java Persistence API (https://www.youtube.com/watch?v=kgHBJksNqnc&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=44)

- We annotate objects and tell JPA how to store objects in the database appropriately to marshall them into rows and columns, and read objects from the database.
```java
@Entity
public class Video {
    //     Tells JPA to consider idNumber as a unique ID for each entry.
    @Id
    //     Tells GPA to generate value for the unique ID on its own.
    @GeneratedValueStrategy = GenerationType.AUTO
    private long idNumber;
    
    private String name;
}
```
3. Entities (Video cannot be found)

4. Spring Repositories (https://www.youtube.com/watch?v=iol6HWrGuOI&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=45)
- To interact with Databases, storing adding and deleting elements. This is possible in JPA by creating a repository.
```java
//    CRUD : Create, Read, Update and Delete. Video is the type of object we are saving, and Long is the type of unique ID that we are using.
public interface VideoRepo extends CrudRepository <Video, Long> {
      
      //    This method will automatically tell JPA to construct a implementation of the given method that can find Videos by Name, and return a List of Videos.
      public List<Video> findByName (String toSearchName) {
      }
      
      //    This method shall automatically return a list of Videos where video.name = toSearchName & video.category = toSearchCategory.
      public List<Video> findByNameAndCategory (String toSearchName, String toSearchCategory) {
      }
}
```
- Many other capabilities are provided by JPA Framework.

5. Understanding SQL Injection Attacks (https://www.youtube.com/watch?v=ZTeGiM2bxFs&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=46)
- The below method's logic can be easily changed using a user's input.
```java
public class VideoSrc {
      public List<Video> myVideos (String name) {
            User currUser = getUser( .. );
            String query = "Select * from video where owner = '" + currUser.getName() + "' and name = '" + name + "'";
            return execute(query);
      }
}
```
- An example would be currUser.getName() returns 'foo' and name provided is "foo' or 'a' = 'a'", the logic becomes : Select * from video where owner = 'foo' and name = 'foo' OR 'a' = 'a'. The second part of logic will provide all of the Videos in the database, and thus release private data, so the attacker's commands are being executed right beside your code.
- JPA's find methods are written so that they can avoid these attacks.

6. Spring Data Code Walkthrough (https://www.youtube.com/watch?v=2vhWuagO_38&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=47)
