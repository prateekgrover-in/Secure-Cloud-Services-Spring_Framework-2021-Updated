This module contains 6 videos:

1. What are Servlets? (https://www.youtube.com/watch?v=IEdij5mwRcs&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=19)

- When browser makes HTTP requests, it goes to "Web Container" which contains one or more Java servlets, based on the type of request one Java servlets handles the request.
- Java technology which handles the incoming web requests is "Servlets". Servlets has methods like "doGet()", "doPost()", "doPut()" and "doDelete()". Based on the type of request method, appropriate servlet method is called.
- Based on the request which servlets to call is identified by "web.xml" file. "web.xml" consists of the routing procedure to call appropriate servlet based on request.

2. Routing & web.xml (https://www.youtube.com/watch?v=55LGF5_Hn5o&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=22)

- Consists of web-app declaration, consists of <servlet> tags, that contain name of servlet, and its class.
- consists of <servlet-mapping> tag, which matches a <url-pattern> to a particular <servlet-name>, all schemes and patterns can be utilized to map servlets based on urls.
- Vast Majority of applications in early 2000s used custom routing techniques in web.xml.

3. A First Cloud Service with a Servlet (https://www.youtube.com/watch?v=1Vq7Jo9ZW8o&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=20)
 
- User is trying to upload a video on the server. Built the servlets that allows this functionality.
- Create a simple class "Video.java". This class contains video metadata such as "Name", "URL" and"videoDuration".
- Create a class "VideoServlets" which extends the "HTTPServlets" class. This class contains "doGet()", "doPost()" methods to define the logic of the code. doGet and doPost methods called based on the type of web request. Based on web request, which method needs to call can be defined in "Web.xml" file.

[i.] doGet() method implementation:

     private List<Video> videoList = new ArrayList<Video>();

        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

            /**
             * Set content type header so client can properly and securely display the content that I send back
             */
            resp.setContentType("text/plain");

            /**
             * This PrintWriter allows us to write data to HTTP response body that is going to be sent to the client.
             */
            PrintWriter sendToClient = resp.getWriter();

            /**
             * Loop through all the stored video in ArrayList and print their name and URL into HTTP response body.
             */
            for(Video v : videoList){
                sendToClient.write(v.getName() + " : " + v.getURL() + "\n");
            }

        }

[ii] doPost() implementation:

        @Override
            protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

                /**
                 * 1. Extract the HTTP request param that are wxpected from URL Query String or URL encoded form body.
                 */
                String name = req.getParameter("name");
                String URL = req.getParameter("URL");
                String durationStr = req.getParameter("duration");

                /**
                 * Convert string duration into number.
                 */
                long durationVideo = -1;
                durationVideo = Long.parseLong(durationStr);

                /**
                 * Set the content type Header so Client's browser know the format of the data and how to interpret it.
                 */
                resp.setContentType("text/plain");

                /**
                 * Validate the query parameters received from the URL. If there is invalid data in request param then Server should send
                 * HTTP 400 Bad Request error to the client.
                 */
                if(name == null || URL == null || durationStr == null || name.trim().length()<1 || URL.trim().length()<1 || durationStr.trim().length()<1){
                    resp.sendError(400,"Invalid Name, URL or Video duration");
                }
                else{
                    /**
                     * If all the string param are valid then create an object of class Video with name, URL and duration.
                     */
                    Video v = new Video("Shiv Dhun","www.youtube.com/shivdhun",120);
                    videoList.add(v);
                }

            }//end of post method.
