Nov10.11.2026



Building Any Rest API endpoints we need to finalize the API request structure, API response structure, API contract, API design\[ What fields we need to pass to stripe, what data we need to provide on that fields, where data comes from, what are the systems involved, how data flows through the system we need to think \& decide then once finalized all design we need to create POJO class that represents how API request structure should be, how API response structure should be, and it helps handle incoming API request structure and handle returning API response structure.



When client sends http request, controller receives the request and it map http request to POJO java object, from the POJO data we can get/retrieve required fields to create an entity java object then we can store this java object in approximate database table. Service process the request and repository perform DB operations and it return responses back to controller.



**Stripe Provider Service: Functionality**



**1.What is Stripe Provider Service?**



It is a microservice responsible for  communicating with the external Stripe Payment gateway.

It has 2 main responsibilities:

&#x20;   \* Send payment request to Stripe.

&#x20;   \* Receive Payment updates from Stripe through webhooks.



**2.Requirements:**



Stripe communication = Handle all communication with stripe inside this service.

Expose an API = Create an API that Payment Validation Service can call.

Receive payment data = Accept the validated payment details from Payment validation service.

Use Rest Client = Use spring Rest Client to send REST API requests to stripe.

Basic Auth =  Use stripe secret key to authenticate requests to stripe.

Webhook endpoint = Creates an endpoints to receive payment status updates from stripe.(notification about payment status after processing)

Stripe CLI = Use this to simulate webhook events during local testing.



**3.Working Flow:**



\-Client sends the request.

\-Payment Validation Service validates payment request.

\-Stripe Provider Service use REST Client to call stripe gateway and before calling it check basic authentication with help of stripe secret API key.

\-After the request is send to Stripe Payment gateway (Here Stripe processes the payment and sends webhook events back to Stripe Provider Service).



Payment Validation Service: Responsible for payment security, field validation, business validation and database operations.

Stripe Provider Service: Responsible for communicating with Stripe and processing webhook events.

========================================================================================================================================================================================================================================================================================================================================================================================



**Stripe Provider Service – REST API Design**



API design = POST /v1/checkout/sessions



**Flow:**

> Payment Validation Service calls the Stripe Provider Service.

> Stripe Provider Service receives the payment details.

> It calls Stripe's Checkout Session API.

> Stripe creates the Checkout Session and returns the response.

> Stripe Provider Service extracts and returns: Unique ID(Stripe Checkout Session ID) \\\& Hosted page URL(Stripe Checkout URL).

> Payment Validation Service can use these details for further processing.



**How did we finalize this API contract?**

Identified the minimum data Stripe needs to create a Checkout Session.

Designed a versioned REST API: POST /v1/checkout/sessions.

Decided to return Stripe's Session ID and hosted checkout URL.



**Simple words:**

=> Payment Validation Service sends payment details → Stripe Provider Service calls Stripe → Stripe creates the Checkout Session → Stripe Provider Service returns the Session ID and hosted page URL.



==========================================================================================================================================================================================================================================================================================================================================================================



**Calling External Stripe PSP: Three Approaches**

**Approach 1: Direct RestClient**

\- Use RestClient directly inside CheckoutSessionService.

\- Handles business logic and HTTP communication together.

\- We must handle timeout, retry, logging and error handling.

**Problem:** Code duplication and difficult maintenance.



**Approach 2: Centralized HttpServiceEngine**

\- Create a common HttpServiceEngine using RestClient.

\- CheckoutSessionService handles only business logic.

\- HttpServiceEngine handles common HTTP logic like timeout, retry, logging and error handling.

**Advantages:** Code reusability and easy maintenance.

**Problem:** Different microservices may need different authentication, timeout and retry settings.



**Approach 3: Service-Specific Clients**

\- Create separate clients for different systems.

\- StripeClient handles Stripe-specific logic.

\- ProcessingServiceClient handles Processing Service-specific logic.

\- HttpServiceEngine handles common HTTP logic.

**Advantages:**

\- Separate responsibilities.

\- Easy maintenance.

\- Service-specific changes won't affect other systems.



Today's learning helped me to understand how to build maintainable microservices by separating business logic and HTTP communication logic.

================================================================================================================================================================================================================================================================================================================================================================================



