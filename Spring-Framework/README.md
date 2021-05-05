Syllabus
The course is organized into the sections outlined below.

Section 1: Talking to the Cloud with HTTP

Module 1: The HTTP Protocol
Introduction
What are Communication Protocols?
Intro to HTTP
Why HTTP?
What is a cloud service?
HTTP Request Methods
HTTP Request Anatomy
URLs Query Parameters
Mime Types Content Type Header
Request Body Encoding
HTTP Response Anatomy
HTTP Response Codes
Cookies
Module 2: Designing Applications with HTTP Communication
Building Cloud Services on HTTP
Protocol Layering / HTTP Design Methodologies
REST
HTTP Polling
Push Messaging



Section 2: Building Java Cloud Services

Module 1: Java Servlets
What are Servlets?
A First Cloud Service with a Servlet
Web.xml
Video Servlet Code Walkthrough
Video Servlet Test Walkthrough with HttpClient
Securely Handling Client Data Avoiding Injection Attacks
Module 2: Better Abstractions for Building Java Cloud Services
Intro to Java Annotations
HTTP to Object Marshalling
Intro to JSON
The Spring Dispatcher Servlet and the Controller Abstraction
Intro to Spring Controllers
Accepting Client Data with RequestParam Annotations
Accepting Client Data with PathVar Annotations
Accepting Client Data with RequestBody Annotations and JSON
Handling Multipart Data
Generating Responses with the ResponseBody Annotation
Custom Marshalling with Jackson Annotations Serializers/Deserializers
Spring Boot Application Structure
Spring Controller Code Walkthrough
Spring Controller Test Code Walkthrough
Module 3: Better Client-side Communication Abstractions
Introduction to Retrofit
Retrofit Client Code Walkthrough
Android Retrofit Client Code Walkthrough
Module 4: Building Loosely Coupled and Extensible Java Services
Spring Dependency Injection Auto-wiring
Spring Configuration Annotations
Spring Dependency Injection Controller Code Walkthrough
Spring Dependency Injection Controller Test Code Walkthrough



Section 3: Building Database-driven Java Cloud Services

Module 1: Persistent Objects
Object to DB Mapping
JPA
Entities
Repositories
Understanding SQL Injection Attacks
Spring Data Code Walkthrough
Module 2: RESTful Services for Persistent Objects
Spring Data REST
Spring Data REST Code Walkthrough



Section 4: Restricting Service Access with User Accounts

Module 1: Secure HTTP Communication
Man in the Middle Attacks Public Key Infrastructure
HTTPS
Module 2: What was I Saying: Keeping Track of Sessions
Sessions
Spring Security Overview
Spring Security Configuration in Java
Building a Custom UserDetailsService
Setting up a custom UserDetailsService
The Principal
Spring Security Role Annotations
More Complex Expression-based Pre Post Authorize Annotations
Spring Security Controller Code Walkthrough
Spring Security Controller Test Code Walkthrough
Module 3: Authenticating Mobile Clients
Stateful Sessions with Cookies Why They Aren't Ideal for Mobile
Stateless Sessions with Tokens
OAuth 2.0
Spring Security OAuth 2.0
A Spring OAuth 2.0 Secured Service
A Retrofit Oauth 2.0 Client for Password Grants




Section 5: Deploying to the Cloud Scaling

Module 1: General Scaling Strategies
Stateless vs. Stateful Applications
Horizontal Scaling
Auto-scaling Horizontally
Caching
Offloading to Cloud Provider Services
Asynchronous IO in Controllers
Module 2: Scaling Up Data Storage
NoSQL Databases
Optimizing for Key-based Lookups
Optimizing for Reads vs. Writes
Contention Sharding
Mongo DB
Spring Data Mongo DB
Database as a Service
Amazon Dynamo
Spring Data Dynamo DB
App Engine Big Table
Module 3: Automating Packaging Deployment
Deploying to Infrastructure as a Service
Deploying to Amazon EC2
Packaging Web Applications into WAR files
Adapting Spring Boot Applications for Google App Engine
Deploying to App Engine
Module 4: Performance Testing
Intro to Cloud Service Performance Testing
Apache JMeter
Building Realistic Tests
