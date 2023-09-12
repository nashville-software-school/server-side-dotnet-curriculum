# Referencing the Same Entity Twice
In this chapter we will cover how to add two properties to an entity that reference the same related entity. This will be important for properly configuring an `Order` in Shepherd's Pies to have `Driver` and `Employee` properties that both reference the `Employee` entity. 

## The `ForeignKey` Attribute
So far we have been relying on naming conventions for EF Core to correctly configure the foreign keys in the databases we have built. For example, the `ChoreAssignment` entity in `HouseRules` has `ChoreId` and `UserProfileId` properties that EF Core can easily understanding as foreign keys to the  `Chore` and `UserProfile` entities. 

However, sometimes foreign keys cannot follow this naming convention, and one common reason for that is that an entity will reference two different rows from the same related table. Here is an example from an application to manage pen pal letters. 

This is the model for a pen pal in the application:
>Pal.cs 
``` csharp
public class Pal
{
    public int Id { get; set; }
    public string Name { get; set; }

}
```
That is very straightforward. But the `Letter` entity will have to reference a `Pal` twice. Once for the writer of the letter, and once for the recipient. This is what that entity looks like:
>Letter.cs
``` csharp
using System.ComponentModel.DataAnnotations.Schema;

public class Letter
{
    public int Id { get; set; }
    public int WriterId { get; set; }
    public int RecipientId { get; set; }
    [ForeignKey("WriterId")]
    public Pal Writer { get; set; }
    [ForeignKey("RecipientId")]
    public Pal Recipient { get; set; }

    public string Content { get; set; }
}
```
You can see above that a `Letter` has both a `RecipientId` and a `WriterId`. Neither of them are called `PalId`, which is the normal naming convention for a foreign key, because if one were called `PalId`, it would be impossible to tell whether it is indicating the writer or the recipient. 

To let EF Core know that both `WriterId` and `RecipientId` refer to instances of the `Pal` entity, `Letter` has two `Pal` properties called `Writer` and `Recipient`. The `ForeignKey` data annotation is used to tell EF Core that another property in the class is the foreign key that can populate the `Pal` property that it matches. 

With these properties configured properly, we are able to add the recipient and the writer to letters when we query the database for them:
``` csharp
app.MapGet("/letters", (PenPalsDbContext db) =>
{
    return db.Letters
    .Include(l => l.Writer)
    .Include(l => l.Recipient)
    .ToList();
});
```

This will return JSON that looks like this: 
``` json
[
  {
    "id": 1,
    "writerId": 1,
    "recipientId": 2,
    "writer": {
      "id": 1,
      "name": "Bob"
    },
    "recipient": {
      "id": 2,
      "name": "Mike"
    },
    "content": "Hi Mike, it's Bob."
  },
  {
    "id": 2,
    "writerId": 2,
    "recipientId": 1,
    "writer": {
      "id": 2,
      "name": "Mike"
    },
    "recipient": {
      "id": 1,
      "name": "Bob"
    },
    "content": "Hi Bob, it's Mike."
  },
  {
    "id": 3,
    "writerId": 3,
    "recipientId": 1,
    "writer": {
      "id": 3,
      "name": "Tony"
    },
    "recipient": {
      "id": 1,
      "name": "Bob"
    },
    "content": "Hi Bob, it's Tony."
  }
]
```

Use this strategy when configuring the `Order` entity in Shepherd's pies to include both a `Driver` (an optional property that indicates who is delivering the order, if it is for delivery), and an `Employee`, which is not optional, and represents the employee that took the order. 