Nov5.11.2026



**1. What is Spring Data JPA?**

Spring Data JPA is a Spring framework module that **simplifies database operations**.

It allows us to **perform database operations** using Java interfaces.

public interface PaymentRepository

&#x20;       extends JpaRepository<Payment, Long> {

}

JPA is a specification for ORM, Hibernate is ORM framework and implementation for JPA, Spring Data JPA is provide automatically write and execute SQL queries for our http methods and no need to write SQL queries manually.



**2. Why do we use Spring Data JPA?**

Without Spring Data JPA, we may need to write SQL queries, manage database connections, and map database results to Java objects manually.

Spring Data JPA reduces this work.

Spring Data JPA reduces database boilerplate code and allows developers to focus more on business logic.



**Benefits:**

\- Reduces boilerplate code.

\- Provides ready-made CRUD methods.

\- Simplifies database operations.

\- Improves code maintainability.

\- Supports pagination and sorting.



**3. What is a Repository?**

A Repository is an interface used to communicate with the database.

It acts as a bridge between our service layer and the database.

**FLOW:**

Client => Receives API requests

Service => Contains business logic

Repository => Performs database operations

MySQL Database => Stores data



**4. What is JpaRepository?**

JpaRepository is an interface provided by Spring Data JPA.

It provides ready-made methods to perform CRUD operations.



Example:

public interface PaymentRepository

&#x20;       extends JpaRepository<Payment, Long> {

}

\- Payment → Entity class.

\- Long → Data type of the entity's primary key.

* We only declare the repository interface. Spring Data JPA creates its implementation automatically at runtime.



**5. CRUD Operations using JpaRepository:**

Operation	Method	  	    Purpose

Create	         save()	        Insert a new record

Read all	findAll()	Retrieve all records

Read by ID	findById()	Retrieve a record

Update	         save()	        Save changes to an entity

Delete	       deleteById()	Delete a record



**6. What happens when execute save method()?**

Our service calls save().

Spring Data JPA handles the repository method.

JPA delegates the persistence operation to Hibernate.

Hibernate generates the required SQL query.

JDBC executes the query.

MySQL stores the data.



