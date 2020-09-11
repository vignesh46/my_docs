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

