This module consists of 13 Videos: 

1. Introduction (3:06) (https://www.youtube.com/watch?v=ov4jGPYWp5U&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=1)

- Estimates that Amazon has 150,000-450,000 Servers that back their infrastructure to support the services that Amazon Supports, similar statistics are obtained with other large companies and also promising startups. 
- What are the special issues that we face when dealing with utilizing Cloud Services?
- Building promising services that can take data from a single input device, send it to cloud, process it and store it.
- The cloud shall have significantly large resources than a single input device and will be able to achieve amazing results when connected a server of 100,000 devices.
- To understand how to build scalable and secure services that can interact with our mobile phones using HTTP Protocol, Spring Framework and OAuth 2.


2. What are Communication Protocols? (4:14) (https://www.youtube.com/watch?v=y3W9gDNZehA&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=2)

- Important to have a robust protocol to interact securely reliably with the resources.
- Example of importance of communication with Mars Rover, about how it would take 2-3 hours to process a message and let it reach the rover.
- Protocol defines how to communicate with a particular entity, it defines syntax, semantics (meaning behind messages) and spells out the timing between messages. 


3. Intro to HTTP (5:17) (https://www.youtube.com/watch?v=AMf7IX1vu9U&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=3)

- HTTP can be defined as a language or an application protocol to communicate between mobile devices and resources.
- Its the way browser communicates with a web server to retrieve and give resources, browsers generates thousands of requests when interacting with a website.
- It is a Client-Server Protocol, A client sends a request to the server, which in turn spends responses in how it was or was not able to process the response.
- Mobile Device or a Browser, or an IoT Device use HTTP to communicate with the server.
- Widely used protocol, as almost all the websites on the internet use HTTP, organizations naturally want to utilize this benefit.


4. Why HTTP? (4:11) (https://www.youtube.com/watch?v=xDFM4dYjT8g&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=4)

- Request obtained by a browser or a mobile device seems exactly the same to a server, HTTP provides a common interface to communicate with any other client.
- Concept of Sessions (remembering Clients and their data), Data Marshalling (converting requests into formats that can be processed), Infrastructure (Load Balancing Techniques), significant investment has been developed in HTTP, thus it is natural to use HTTP as an interface between the client and server.
- However, some things like an email received can be difficult to fit into an HTTP Model.


5. What is a Cloud Service? (7:45) (https://www.youtube.com/watch?v=AKWnsW8vvqE&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=5)

- The variety of services (Applications) delivered to customers on demand over the internet.
- eg: Creating an account of website - Facebook. User passing his data, facebook cloud service receive that data, validate the data if needed and store it in the server (cloud storage). This service / application is on-demand (user can use anytime).
- The company does not need to built hardware. They deploy the application on cloud.


6. HTTP Request Methods (4:45) (https://www.youtube.com/watch?v=K1OcErpdE-8&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=6)

- In HTTP (Hyper Text Transfer Protocol), always client sends the request to the server.
- Each request has "Request Method" which specify the server what exactly a user looking for.
- Based on the request User provides, server will respond with resources. for eg: A user requested for https://google.com and server responded with the webpage.
- Here are four main request methods.
    i. GET      :   Read Operation
    ii. POST    :   Write Operation (Create a user; sign up form)
    iii. PUT    :   Update operation
    iv. DELETE  :   Delete operation.


7. HTTP Request Anatomy (4:40) (https://www.youtube.com/watch?v=DvQp7hJk0TA&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=7)

Request Line (Mandatory)
- A user sends Request Line = Request Method + Resource (path).
- Server needs to know kind of request it is and what is the resources user looking for.
- Resource is usually a path in the request line. For eg: google.com/jeans/aeropostale/signup.

Headers (Optional to get in Required Format)
- User needs to send the data on a specific manner. Same way, server needs to respond the data in a proper manner.
- This is defined in the "Header" - "Extra information to help server identify the response type. "
- Header includes below information.
    i. language     : which language to provide response.
    ii. Character-Set : unicode, utf-8 etc. 
    iii. content-type   : what type the response should return for eg: application/json
    iv. cookies: A small amount of data server sends along with the response. This saves user name, sends the user activities back to server for better serve next time.

Body (Mandatory in cases where Required)
- It is core data sends to server.
- Body includes JSON payload. for eg, at the time of user sign up to a website, User information is passed in the form of JSON Payload to the server.


8. URLs & Query Parameters (8:45) (https://www.youtube.com/watch?v=7D0031eA-ZQ&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=8)

- Uniform Resource Locator (http://host::port/path).
- Query Parameters : for eg: https://www.google.com?city=denver&age=25
- Here, anything after "?" is called Query parameters. key1 = city, value1 = denver, key2 = age,  value2 = 25. & defined for multiple key value pairs.
- These allow passing extra information with our request. We cannot have certain special characters in these query parameters, as the server would not be able decode these values
- URL Encoding is used to pass special characters with getting in specific format for Server for it to be able to understand the queries.


9. Mime Types & Content Type Headers (7:54) (https://www.youtube.com/watch?v=FBkZ2TJZZUY&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=9)

- When server and user sends data, it could be of different format (xml, json, raw text ect.).
- Along with the data, user/server needs to specify what is the format of the data.
- MIME type is the identifier of type of format which included in the body. Common MIME types are: image/jpg, image/png, text/plain, text/html, application/json, application/xml.
- These types allow server to know how to interpret the information. "Content-type" header provides MIME type and therefore facilitates the communication of response and requests. 


10. Request Body Encoding (5:55) (https://www.youtube.com/watch?v=zclWzlAnRwA&list=PLyUlngzGzkztgTizxM6_zqiw8sRj7vBm0&index=10)
- 
