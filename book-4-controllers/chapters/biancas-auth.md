# :bike: Authentication and Authorization
In this chapter we will explore the concepts of authentication and authorization, and how they are implemented in Bianca's Bike Shop.

## Definitions

**Authentication** - proving you are who you say you are.

**Authorization** - proving you are allowed to do what you're trying to do.

### :cookie: Cookies
This application (and the applications that we're going to build for the rest of the course) use cookies to facilitate authentication and authorization. 
``` mermaid
sequenceDiagram
    participant C as Client
    participant A as API
    participant D as DB
    C->>A: email + password;
    activate C
    activate A
    A->>D: retrieve hashed pw from db
    activate D
    D-->>A: check provided password against hash
    deactivate D
    A-->>C: cookie if correct pw, otherwise 401
    deactivate C
    deactivate A
    loop requests during session
        C->>A: other request with cookie
        activate A
        activate C
        A-->>C: Cookie not present or invalid - 401
        A->>D: SQL query to Postgresql if cookie is valid
        activate D
        D-->>A: return data
        deactivate D
        A-->>C: 200 with data
        deactivate A
        deactivate C
    end
    C->>A: logout request
    activate C
    activate A
    A-->>C: API response with expired cookie (browser will discard it)
    deactivate A
    deactivate C
```

The diagram above shows the basic workflow:
1. The client sends an HTTP request with the email and password
1. The API receives that request, and requests the hashed password that matches the email from the database. 
1. The API compares the hashed password to the provided password
    - If they match, the API returns 200 with the cookie on the `set-cookie` header of the HTTP response:
        ```
        set-cookie: BiancasLoginCookie=CfEv...httponly (rest of cookie omitted)
        ```
    - If they don't match, the API returns 401 (Unauthorized)
1. Once the browser gets the cookie, it saves it, and will send it back to the API with future requests. This is how the API can know that the request it is getting is from a client that has logged in. 
1. When the client send a request to log out, the API will send back an expired cookie on the `set-cookie` header:
    ```
    set-cookie: BiancasLoginCookie=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/; secure; samesite=strict; httponly
    ```
     Notice this cookie doesn't even have any data in it (`BiancasLoginCookie=;`). It is also expired: `expires=Thu, 01 Jan 1970`. Browsers discard cookies that are expired, so the cookie will be deleted from the browser's cookies. Without a cookie, the browser is not logged in anymore!

#### What's in the cookie?
The cookie has a bunch of information encoded in it (it will look like a random string of characters in the dev tools). It is possible to programmatically add different data to the cookie (each piece is called a "claim"). Bianca's Bike Shop is configured to include the following:
1. User Id
1. UserName
1. Email
1. Roles (if any)

Whenever the client makes a request to the app, the API uses the cookie to determine:
1. That the user is logged, just by the presence of a valid cookie (authentication)
1. What Email is associated with the login, to see _who_ the user is (authentication)
1. What _roles_ the user is in, which say what they are allowed _to do_.  (authorization)

## BiancasBikesDbContext
There are a few major changes to the DbContext class that you will interact with:
1. `BiancasBikesDbContext` inherits from the `IdentityDbContext<IdentityUser>` class, rather than from `DbContext`. `IdentityDbContext` comes with a number of extra models and tables that will be added to the database. They include:
    - `IdentityUser` - this will hold login credentials for users
    - `IdentityRole` - this will hold the various roles that a use can have
    - `IdentityUserRole` - a many-to-many table between roles and users. These define which users have which roles. 
1. In `OnModelCreating`, we are seeding the database with rows in the above tables. For example:
    ``` csharp
    modelBuilder.Entity<IdentityUser>().HasData(new IdentityUser
        {
            Id = "dbc40bc6-0829-4ac5-a3ed-180f5e916a5f",
            UserName = "Administrator",
            Email = "admina@strator.comx",
            PasswordHash = new PasswordHasher<IdentityUser>().HashPassword(null, _configuration["AdminPassword"])
        });
    ```
    - The `Id`s for the Identity Framework tables are _Guids_, not `ints`. A `Guid` (Global Unique Identifier) can be generated with `Guid.NewGuid()`. You will need to do this when you create your own data to seed. 
    - The password is being _hashed_ before storage in the database, and we are retrieving it from the _user-secrets_ so that it is not stored in the GH repository. The details of password hashing are beyond the scope of the course. The only thing you should know is that hashing the password obscures the actual password in the database for security reasons. 

## Examples from the codebase
> AuthController.cs
``` csharp
[HttpGet("Me")]
[Authorize]
public IActionResult Me()
{
    var identityUserId = User.FindFirstValue(ClaimTypes.NameIdentifier);
    var profile = _dbContext.UserProfiles.SingleOrDefault(up => up.IdentityUserId == identityUserId);
    var roles = User.FindAll(ClaimTypes.Role).Select(r => r.Value).ToList();
    if (profile != null)
    {
        profile.UserName = User.FindFirstValue(ClaimTypes.Name);
        profile.Email = User.FindFirstValue(ClaimTypes.Email);
        profile.Roles = roles;
        return Ok(profile);
    }
    return NotFound();
}
```
This is an endpoint from the `AuthController` that gets the userProfile for a logged in user (a common use case that most front-end applications need). The `[Authorize]` attribute on this method tells the framework to require a cookie in order to access this resource. A request without a cookie will get an automatic `401` response. The `User` property is inherited from `ControllerBase`, and contains all of the data from the cookie (each of the "claims"). In this case, this method is getting the user Id (to look up the user's `UserProfile`) as well as all of the roles for that user.

> UserProfileController.cs
``` csharp
[HttpGet]
[Authorize(Roles = "Admin")]
public IActionResult Get()
{
    return Ok(_dbContext.UserProfiles.ToList());
}
```
This method from the `UserProfileController` gets the data for all users. This data should only be available to Admin users (it will be used to terminate employees, hire new ones, as well as upgrading an employee to be an Admin). `[Authorize(Roles = "Admin")]` ensures that this resource will only be accessible to authenticated users that also have the `Admin` role associated with their user id. A logged in user that is not an Admin will receive a `403` (Forbidden) response when trying to access this resource.

## Other Auth-Related Code in the Codebase
The following files and modules contain the code related to auth:
1. `AuthController.cs`
1. `Program.cs`
1. `authManager.js`
1. `AuthorizedRoute.jsx`

You are _not responsible_ for the auth-related code in those modules, but you are expected to be able to _use_ them properly. In this book you will see multiple examples of how to do so, but you do not need to know _how the code works_, as it is outside the scope of the course.

Up Next: [Bianca's Tour Part III: Dependency Injection](./biancas-dependency-injection.md)

## 🔍 Additional Materials

1. [Cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)