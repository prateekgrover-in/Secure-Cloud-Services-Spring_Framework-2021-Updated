This module contains 5 videos: 



1. Building Cloud Services on HTTP (https://www.youtube.com/watch?v=xVvGud1xh6E&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=14)

- Communication Protocol that can be used to communicate with resources at the cloud can be HTTP, For eg: PUT command can be used to store a video, GET can be used to fetch the video from the resource as an HTTP Response.
- We can encode data for a video inside a multipart Request Body, which includes the video, name as a key-value pair, content-type.
- Thus HTTP Constructs that we learned can be used to send request using Multipart or URL encoded bodies.
- Each video might have a URL, its path on the server, which can be used to retrieve the video, HTTP Repsonse can have a response code, giving us result of our request. Response can have Response Body which can have a video or text information.
- Thus HTTP is providing a framework to work with servers in the Cloud Service.



2. Protocol Layering & HTTP Design Methodologies (https://www.youtube.com/watch?v=cV1fyArK2_s&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=15)

- HTTP Protocol is layered over the TCP/IP Protocol, while our Video Storing Protocol can be layered over the HTTP Protocol, thus HTTP can be just a single protocol layer used to build services.
- As we can observe there are different design choices that are taken into designing an application, which can also be done by building protocols on top of HTTP.
- Web Services like WSDL or SOAP or another set of Rules are REST, but these definitions have been watered down, so these definitions are currently used in a very loose manner.
- We'll be building a service which are accessible over HTTP which can have REST-Like Properties or a web-service-like properties.



3. What is REST? (https://www.youtube.com/watch?v=e6h87rzeGJE&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=16)

- Interacting with resources rather than specific objects, common format to build URLs in order to access resources. 
- A way to represent a video {v_id} would be GET /videos/{v_id}/duration, this would refer to getting the duration of {v_id} video.
- Another would be GET /videos, where we would expect to get a list of videos.
- Typically, a URL Addressing Scheme like the one defined above is what people refer to as REST.
- Another example would be PUT /videos/{v_id} would be create/update a resource {v_id}, POST /videos/ would add an ID and add the video to it.



4. HTTP Polling (https://www.youtube.com/watch?v=OsgrJDMPl58&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=17)

- HTTP is client Driven protocol i.e. Client is initiating the communication. Client sends a request and server responded.
- Server received updates but can not send to client until client requested. Client needs to decide how frequent request needs to send to server for update. Refresh every time it wants to receive updates to its data.
- One of the method is - Polling. Client requests server for an update at every T seconds (regular interval) . Even if there is no update, server allocated the resources, check for the update and sends the "no update" response. There can be long durations when there are no updates, and thus can lead to wastage of resources.
- Antoher method can be if the client does not receive any update in T seconds then client exponentially increase the time gap between two requests to 2T, 4T, 8T etc.

Web Sockets

- Another approach for Client - server connection is "Web Socket".
- Here, Client sends request to the server and established Web socket. Web socket is bi-direction communication bridge i.e. either client or server sends the data to other party. Thus, Web sockets are not completely client driven but both the parties can initiate the communication.
- One of the issue is when client lost the internet connectivity then web socket connection went down. Client needs to send request to server and establish the web socket connection. Client should have mechanism to reestablish the socket connection at the time of disconnection.



5. Push Messaging (https://www.youtube.com/watch?v=KNhRvFLq0Ao&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=18)

- Every Android Client sets up a persistent connection to Google Connection Servers with XMPP Protocols, the android client gets a unique registration ID, which the client send s to the server to process requests, thus the server would now know the Reg. ID.
- This allows the server to send a POST message to the GCM Server with Reg ID, the message thus can be forwarded by Google's Infrastructure to the Client. 
- So, Android Libaries can connect automatically whenever the connection goes down, and push notifications can be sent without the problem of disconnection.
- The messages that can be sent through Google's Servers are limited to 4 kB, so only small amounts of information can be sent ie. a new subscriber on your YouTube Channel
- Push-to-Sync Model : The client can receive the notification that it needs to update, and then it polls to receive updating information form the server, this also allows control of security as no information is shared to a third-party.
