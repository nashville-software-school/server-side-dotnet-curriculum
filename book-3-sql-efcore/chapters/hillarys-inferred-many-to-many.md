# Appointment Services
In this chapter you will see another way of creating many-to-many relationships with EF Core

## Connecting the `Service` and `Appointment` entities
Many `Service`s can be provided during an `Appointment`, and  many `Appointment`s can provide the same service, which means that there is a many-to-many relationship between these entities. 

In the database (and the ERD), this is represented with a join table, as you have seen before. It is possible to create a C# class that represents this table as well:
``` csharp
public class AppointmentService
{
    public int Id { get; set; }
    public int AppointmentId { get; set; }
    public int ServiceId { get; set; }
}
```

EF Core will notice that the table has `AppointmentId` and `ServiceId` and create foreign keys to the `Appointment` and `Service` tables. This works fine!

However, it is also possible for EF Core to infer this many-to-many relationship without an intervening class:
> Appointment.cs
``` csharp
public class Appointment
{
    // the rest of the class properties are omitted
    public List<Service> Services { get; set; }
}
```
> Service.cs
``` csharp
public class Service
{
    // the rest of the class properties are omitted
    public List<Appointment> Appointments { get; set; }
}
```

EF core will notice that a `Service` has many (a List!) `Appointment`s, and will notice that an `Appointment` has many `Service`s for the same reason. In the database it will still create an `AppointmentService` table, but you do not need to represent it in your C# code with its own class. Neat!

### Seeding Data for Inferred Many-To-Many Relationships
Because there is no `AppointmentService` class in your program, you will have to seed data for that table in your database slightly differently, using a string to identify the table. Here is an example that would work for Hillary's Hair Care:
``` csharp
modelBuilder.Entity("AppointmentService").HasData(new object[]
{
    new {AppointmentsId = 1, ServicesId = 1},
    new {AppointmentsId = 1, ServicesId = 6},
    new {AppointmentsId = 2, ServicesId = 2},
    new {AppointmentsId = 2, ServicesId = 3},
    new {AppointmentsId = 3, ServicesId = 1},
    new {AppointmentsId = 4, ServicesId = 4}
});
```

## Limitations
This alternate way to define a many-to-many relationship has limitations:
1. It will prevent duplicates of the same relationship because it uses the two foreign keys as a composite primary key:
    ``` sql
    CREATE TABLE AppointmentServices (
        ServiceId integer NOT NULL,
        AppointmentId integer NOT NULL
        PRIMARY KEY (ServiceId, AppointmentId) 
    );
    ```
    The consequence of this is that only one unique combination of `ServiceId` and `AppointmentId` is allowed in the `AppointmentServices` table. This is either a good or bad thing, depending on your app's functionality. If you need to allow duplicates, you are better off creating a class in C# to allow each `AppointmentService` to have its own `Id`. In this project, you do _not_ need to allow duplicates, so you can use either method.
1. If your many-to-many relationship table needs a _payload_, that is, other data about the relationship besides the foreign keys, you need to create a C# class to represent the join table. For example, if you have a product ordering app, it might have entities like this:
    > Product.cs
    ``` csharp
    public class Product
    {
        public int Id { get; set; }
        public string ProductName { get; set; }
        public decimal Price { get; set; }
    }
    ```
    > Order.cs
    ``` csharp
    public class Order
    {
        public int Id { get; set;}
        public DateTime OrderDate { get; set; }
    }
    ```
    > OrderProduct.cs
    ``` csharp
    public class OrderProduct
    {
        public int Id { get; set; }
        public int OrderId { get; set; }
        public int ProductId { get; set; }
        public int Quantity { get; set; }
    }
    ```
    In this case, the `OrderProduct` class has a _payload_, the `Quantity` property, which we want to be able to access in our queries. Here, too, we should use a separate C# class to define the relationship. 


There are _many_ ways to configure many-to-many relationships in EF Core. Here we have presented only two, which should suffice for the work you are doing in the course. 

## 🔍 Additional Materials
1. [Multi-Column Primary Key](https://www.postgresql.org/docs/current/ddl-constraints.html#DDL-CONSTRAINTS-PRIMARY-KEYS)
1. [Many-To-Many in EF Core](https://learn.microsoft.com/en-us/ef/core/modeling/relationships/many-to-many)