# HIBERNATE NOTES

A framework for persisting/ saving java objects in database

> ORM (Objectes to Relational Mapping)

## CRUD
- Create
- Read
- Update
- Delete

## Entity Class
Java class that is mapped to a database table

> Use javax.persistence package(JPA) for Hibernate related annotations. Hibernate package is an implementation of the JPA.
> The Hibernate team recommends the use of JPA annotations as a best practice.

## Session Factory
- Reades the hibernate config file
- Create Session objects
- Heavy weight object
- Only create once in your app

```java
SessionFactory factory = new Configuration().configure("hibernate.cfg.xml").addAnnotatedClass(Student.class).buildSessionFactory();
```

## Session
- Wraps a JDBC connection
- Main object used to save/retrive objects
- Short lived object
- Retrieved from SessionFactory

```java
Session session = factory.getCurrentSession();
```

> HQL: Hibernate Query Language

## Annotations
**@Entity** - Helps to map the class to Database Table

**@Table(name = "table_name")** - Refers to a specified table

**@Id** - Refer a filed as a primary key

**@Column(name = "id")** - Map field to a specified column

## Working with Data sample
```java
@Column(name="date_of_birth")
@Temporal(TemporalType.DATE)
private Date dateOfBirth;
```

## To save
```java
session.save(student1);
```

## Fetch Data
```java
//Fetch using Id
Student result = session.get(Student.class, student1.getId());
//Fetch all records using Query
List<Student> students = session.createQuery("from Student").getResultList();
//Fetch records using Query with filter conditions
List<Student> students = session.createQuery("from Student where lastName='Amal'").getResultList();
```

## Update
```java
//Update using ID
session.beginTransaction();
Student result = session.get(Student.class, sId);
result.setLastName("Prasanth");
session.getTransaction().commit();

//Update using Query
session.beginTransaction();
session.createQuery("update Student set email='test@gmail.com'").executeUpdate();
session.getTransaction().commit();
```

## Delete:
```java
//Delete using object
session.delete(result);

//Delete using Query
session.beginTransaction();
session.createQuery("delete from Student where id='5'").executeUpdate();
session.getTransaction().commit();
```
## Cascade
Apply same operation to related entities. save/Delete one table will reflect in their linked tables.

## Eager
will retrieve related entities too

## Lazy
Will retrieve related entities on request

## UniDirectional
Retrieve from one direction.

Car -> CarDetails

## BiDirectional
Retrieve from both directions.

Car -> CarDetails

Car <- CarDetails



