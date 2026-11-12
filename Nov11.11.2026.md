**Nov11.11.2026**





Today i learned how to use spring Rest Client to make outgoing HTTP calls and successfully integrate our Stripe Provider Service with Stripe PSP.



**1.Layered Architecture:**



Controller = Exposes REST API for payment validation service.

Service = Handles request and response processing for Stripe Create Session API.

Stripe Client = Handles outgoing API calls specifically to Stripe.

Http Service Engine = Common class for making outing HTTP calls to different services.

**Flow:** Controller -> Service -> Stripe Client -> Http Service Engine -> Stripe PSP



**Note: Eat the Frog First** 

Always start with the most important and uncertain task first.

> Identify the most important task.

> Implement and test it first.

> Find possible problems early.

> Solve problems before proceeding with other tasks.

**In our project:**

Our biggest uncertainty is whether our Spring Boot application can successfully call Stripe PSP. So, first we make an actual Stripe API call using Rest Client. Once it works, we improve the code towards production.



**2. Rest Client vs Fluent API vs Builder Design Pattern:**



Rest Client is a spring framework HTTP client used to communicate with other applications through HTTP.

It is used for sync HTTP calls. It is available through spring web. It supports Fluent API(method chaining)



Fluent API means calling multiple methods through method chaining in readable manner.

Instead of calling each methods separately, we chain them together.



Builder Design Pattern used to create objects step by step. 

Instead of passing many values through a constructor, we configure the object using builder methods and finally call build().



**3. Rest Client Configuration:**

Spring provides an auto-configured RestClient.Builder with sensible defaults.

We use this builder to create our RestClient object.

**Implementation flow:**

1\. Create AppConfig.

2\. Create a RestClient bean using RestClient.Builder.

3\. Inject the RestClient into HttpServiceEngine.

4\. Use it for outgoing HTTP calls.



**4. Http Service Engine: Handles how to make the HTTP call. The service layer decides what HTTP call needs to be made.**

It contains common logic for making outgoing HTTP API calls to different services.

**Method			Simple meaning**

method()	Specifies HTTP method (GET, POST, PUT, DELETE).

uri()		Specifies the API URL.

headers()	Sets request headers.

body()		Sets request body/data.

retrieve()	Executes the HTTP request and prepares to handle the response.

toEntity()	Gets the response as a Response Entity (body, status, headers).



**5.Http Headers vs Consumer vs Lambda**

**HTTP Headers** provide additional information about an HTTP request.

**Basic Auth:** Sends username and password in the Authorization header using Base64 encoding. In Stripe, username is Secret Key and password is empty.



**Consumer** is a functional interface that accepts one input and performs an operation without returning anything.

**Example:** Consumer<String> c = value -> System.out.println(value);



**Lambda** is a shorter way to implement the single abstract method of a functional interface.

**Syntax:** parameter -> implementation





**6. Implementation:**

**Complete the Stripe API integration using RestClient.**

1\. Create RestClient bean in AppConfig.

2\. Inject RestClient into HttpServiceEngine.

3\. Prepare HTTP headers using HttpHeaders.

4\. Configure Basic Authentication.

5\. Set Content-Type to application/x-www-form-urlencoded.

6\. Prepare Stripe request body using MultiValueMap.

7\. Make a POST request to Stripe Create Session API.

8\. Retrieve the response using ResponseEntity.

9\. Test through Postman.

Expected result: Stripe should return a successful response containing:

\- Unique Session ID.

\- Hosted Page URL.

===================================================

Basic Authentication is working.

Request headers are correct.

Request body is accepted by Stripe.

Stripe Create Session API is responding successfully.

===================================================

This was a great hands-on learning experience in understanding how Spring Boot applications communicate with external services.

