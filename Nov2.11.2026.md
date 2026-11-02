**Nov2.11.2026**





**Revise(Git \& GitHub workflows): Understand end-end IT release process**



All project code is git code , all git code is put into repo, this repo is available/host in GitHub/bit bucket.



Create repo in bitbucket, create branches(main, integration, feature branch). = Bit Bucket/ GitHub.(cloud remote repo hosting services).



clone remote repo in our local machine(local repo), work \& test then add all project files(stage), commit(changes locally saved in local repo), then push into remote repo. Now, all project code is available in remote repo. = Source Tree/GitHub desktop.(GUI for working with git repositories).



Get the code from feature branch and deploy in AWS(dev environment). For basic testing.



Make pull request and code review happen and merge the code in integration branch. After the integration branch they will deploy in test/QA env for testing the feature(finding any bugs).



If this feature is perfectly good then again make pull request and code review happen and merge the code in main branch. After the main branch they will deploy in prod env for release the feature to the end-user.(final testing also happen UAT env)



**Feature Development Workflow : The goal is all completed sprint features should be available in the integration branch.**

1\. Create Feature Branch

2\. Local Development \& Testing

3\. Push \& DEV Testing

4\. Pull Request \& Code Review

5\. Merge into Integration

6\. Repeat for All Developers

From Integration → Feature Branch → Local Testing → AWS DEV Testing → PR \& Code Review → Integration

* **Steps to work on bugs filed by Testers:**
* Reproduce and find the root cause in QA env.
* If not able to fully pinpoint the come to local features branch and test with breakpoints, logs, and find root cause.
* Once root cause is found, then fix it in feature branch in local machine.
* Push to features branch in bit bucket \& deploy in AWS => complete code review and merge, get changes to integration branch.
* Deploy integration to QA, and verify.
* Update Jira and inform to QA for verification.



**Note: (Bugs fixing)**

Understand the bugs , find bugs, test with small dummy data like put logs how it working then we put logic then test it again how its flow working like wise we need to fix bugs.

======================================================================================================================================================================================

**Understand the complete IT release management:**



**IT release management** = controls how approved code is moved from dev to prod safely.

It is the process of planning, developing, testing, approving and deploying software changes from the dev to the prod env.

whenever developers make any changes to an application, those changes should be properly tested and approved before releasing them to real customers.



**=>** **Understand the environments: DEV → QA → UAT → PROD**

1. DEV – Development Environment(Developers develop new features and perform initial testing.)
2. QA – Quality Assurance(QA testers test the application and identify bugs.)
3. UAT – User Acceptance Testing(Customers verify whether the application meets their requirements.)
4. PROD – Production Environment(Application is available to real customers)



**=> Understand Git branches:**

* Parallel development; Track all code changes; Collaboration with team members.
* Project code no loss even though if our system is in difficult situation. Because our code is available in cloud environment.
* Where to cut the feature, from which branch and where to deploy this much knowledge as needed.
1. Main Branch = contains stable and approved code.
2. Integration Branch = Developers merge their completed features into this branch.
3. Feature Branch = Different developers work on different features.
4. Hotfix branch = Some critical issues happen in prod so developers immediately create hotfix branch from main then fix the issues, review it, merge the fix back into the main branches.
5. Release branch = When features are ready to release, need to create release branch and it is used to prepare particular software release.(In UAT any issues find, then issues fixes related things happen in this branch)



**=> Complete release management workflow: Git branches and deployment environments are two different things.**

1. Create feature branch from integration = Develop a new feature using VS Code
2. Develop code and test locally = Developer saves the changes into the local Git repository.(git add, git commit)
3. Push code to Bitbucket = Developer pushes the feature branch to the remote Bitbucket repository(git push)
4. Create Pull Request \& Code Review = Senior developer reviews the code and approves it.
5. Merge into Integration branch = Combine multiple developers' features.
6. Deploy to DEV environment = The integrated code is deployed to the dev env.(Developers perform initial testing.)
7. Deploy to QA environment = The tested code is moved towards QA.(QA testers Perform functional testing \& Finding bugs)
8. Create Release branch = Once the features are ready, a release branch is created.(Prepared for final testing.)
9. Deploy to UAT = Business users test the application and verify whether the functionality meets their expectations.
10. Merge into Main = After successful testing and required approvals, the release changes are merged into main.
11. Deploy to Production = The approved release is deployed to production.(User can use the updated application.)



Feature → Local Testing → Push → PR \& Code Review → Merge into Integration → DEV → QA → Create Release → UAT → Main → Production

==========================================================================================================================================

==========================================================================================================================================



**Error Handling Implementation( 29.10.2026 we discussed theory) - Step by Step**

**Step 1: Create ValidationException**



public class ValidationException extends RuntimeException {



&#x20;   private String errorCode;

&#x20;   private String errorMessage;

&#x20;   private HttpStatus httpStatus;



&#x20;   public ValidationException(

&#x20;           String errorCode,

&#x20;           String errorMessage,

&#x20;           HttpStatus httpStatus) {



&#x20;       super(errorMessage);



&#x20;       this.errorCode = errorCode;

&#x20;       this.errorMessage = errorMessage;

&#x20;       this.httpStatus = httpStatus;

&#x20;   }



&#x20;   public String getErrorCode() {

&#x20;       return errorCode;

&#x20;   }



&#x20;   public String getErrorMessage() {

&#x20;       return errorMessage;

&#x20;   }



&#x20;   public HttpStatus getHttpStatus() {

&#x20;       return httpStatus;

&#x20;   }

}

==========================================================================================================================================

**Step 2: Throw the exception when an error occurs**



if (firstName.contains("TEMP")) {



&#x20;   throw new ValidationException(

&#x20;       "10001",

&#x20;       "First name cannot contain TEMP",

&#x20;       HttpStatus.BAD\_REQUEST

&#x20;   );

}

==========================================================================================================================================

**Step 3: Create ErrorResponse: Defines the standard error response structure returned to the API invoker.**



public class ErrorResponse {



&#x20;   private String errorCode;

&#x20;   private String errorMessage;



&#x20;   public ErrorResponse(

&#x20;           String errorCode,

&#x20;           String errorMessage) {



&#x20;       this.errorCode = errorCode;

&#x20;       this.errorMessage = errorMessage;

&#x20;   }



&#x20;   public String getErrorCode() {

&#x20;       return errorCode;

&#x20;   }



&#x20;   public String getErrorMessage() {

&#x20;       return errorMessage;

&#x20;   }

}

==========================================================================================================================================

**Step 4: Create GlobalExceptionHandler**



@RestControllerAdvice

public class GlobalExceptionHandler {



&#x20;   @ExceptionHandler(ValidationException.class)

&#x20;   public ResponseEntity<ErrorResponse> handleValidationException(

&#x20;           ValidationException ex) {



&#x20;       ErrorResponse errorResponse = new ErrorResponse(

&#x20;               ex.getErrorCode(),

&#x20;               ex.getErrorMessage()

&#x20;       );



&#x20;       return new ResponseEntity<>(

&#x20;               errorResponse,

&#x20;               ex.getHttpStatus()

&#x20;       );

&#x20;   }



&#x20;   @ExceptionHandler(Exception.class)

&#x20;   public ResponseEntity<ErrorResponse> handleGenericException(

&#x20;       Exception ex) {



&#x20;       ErrorResponse errorResponse = new ErrorResponse(

&#x20;           "10000",

&#x20;           "Something went wrong. Please try again later."

&#x20;       );



&#x20;       return new ResponseEntity<>(

&#x20;           errorResponse,

&#x20;           HttpStatus.INTERNAL\_SERVER\_ERROR

&#x20;       );

&#x20;   }

}

==========================================================================================================================================

==========================================================================================================================================

==========================================================================================================================================

**What is HTTP Status?**

HTTP status is a numeric code sent by the backend to indicate the result of an HTTP request.



There are five main categories:

Series	  Meaning

1xx	Informational

2xx	Success

3xx	Redirection

4xx	Client error

5xx	Server error



Important status codes:

Code	Meaning		When to use

200	OK		Request successful

201	CREATED		New resource created

400	BAD REQUEST	Invalid request

401	UNAUTHORIZED	Authentication required or invalid

500	INTERNAL SERVER ERROR	Unexpected server-side error



==========================================================================================================================================

==========================================================================================================================================



**Success Flow**

Client → Controller → Service → Successful processing → Controller → Client

Controller returns the success response.



=> Success response comes from controller(@RestController). Error responses comes from @RestControllerAdvice.



**Error Flow**

Client → Controller → Service → Error → Throw Exception → GlobalExceptionHandler → Client

GlobalExceptionHandler prepares and returns the error response.



==========================================================================================================================================

==========================================================================================================================================

**Note:**

throw – Used to throw an exception.

throws – Used to declare that a method may throw an exception.

RuntimeException – Base class for unchecked exceptions.

@RestControllerAdvice – Provides centralized exception handling for controllers.

@ExceptionHandler – Specifies which exception a method handles.

ResponseEntity – Allows us to return both response body and HTTP status.



=======================================================================================================================================

==========================================================================================================================================



**Understanding of How error response return to invoker:**

* We create a custom exception class with errorCode, errorMessage, httpStatusCode.
* When an error occurs in our application, we throw a custom unchecked exception containing errorCode, errorMessage, and httpStatus.
* Spring's exception-handling mechanism catches the exception and looks for a matching handler in @RestControllerAdvice.
* Inside GlobalExceptionHandler, we prepare a standard ErrorResponse and return it to the API invoker with the appropriate HTTP status.
* This approach helps us maintain a consistent error response structure across the entire application.



=======================================================================================================================================

==========================================================================================================================================



