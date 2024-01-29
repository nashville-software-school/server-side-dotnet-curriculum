# Managing Employee Roles
Most applications that have users need to manage what those users can do. Some users need admin privileges, while others may not need access to see everything, let alone edit anything (read-only access). This chapter will demonstrate a strategy for managing and using a role-based access control (RBAC). 

## The `AuthorizedRoute` component
Most of the route elements in the application are going to be wrapped with the `AuthorizedRoute` component that has been created for this repository. The component accepts an array of role names as a prop, and will check if the logged-in user has any of those roles. if `all` is provided as another option, as in `<AuthorizedRoute roles={["Admin", "Employee"} all>`, then the `AuthorizedRoute` will only allow access if _all_ of the listed roles are true of the logged-in user. If no roles are provided, the user must merely be logged in to view the route. 

The Employees menu item links to a route that requires admin privileges:
``` jsx
<Route
    path="employees"
    element={
    <AuthorizedRoute roles={["Admin"]} loggedInUser={loggedInUser}>
        <p>Employees</p>
    </AuthorizedRoute>
    }
/>
```
Create a new component for this route called `UserProfileList` in a new `components` folder called `userprofiles`:
> components/userprofiles/UserProfileList.jsx
``` javascript
import { useEffect, useState } from "react";
import { Button, Table } from "reactstrap";
import { Link } from "react-router-dom";
import { getUserProfilesWithRoles } from "../../managers/userProfileManager";

export default function UserProfileList({ loggedInUser }) {
  const [userProfiles, setUserProfiles] = useState([]);

  useEffect(() => {
    getUserProfilesWithRoles().then(setUserProfiles);
  }, []);

  const promote = (id) => {
    console.log(`${id} promoted!`);
  };
  const demote = (id) => {
    console.log(`${id} demoted!`);
  };

  return (
    <>
      <h2>User Profiles</h2>
      <Table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Address</th>
            <th>Email</th>
            <th>Username</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          {userProfiles.map((up) => (
            <tr key={up.id}>
              <th scope="row">{`${up.firstName} ${up.lastName}`}</th>
              <td>{up.address}</td>
              <td>{up.email}</td>
              <td>{up.userName}</td>
              <td>
                {up.roles.includes("Admin") ? (
                  <Button
                    color="danger"
                    onClick={() => {
                      demote(up.identityUserId);
                    }}
                  >
                    Demote
                  </Button>
                ) : (
                  <Button
                    color="success"
                    onClick={() => {
                      promote(up.identityUserId);
                    }}
                  >
                    Promote
                  </Button>
                )}
              </td>
            </tr>
          ))}
        </tbody>
      </Table>
    </>
  );
}
```

Make the component the element for the `Employees` menu item:
``` jsx
 <Route
    path="employees"
    element={
    <AuthorizedRoute roles={["Admin"]} loggedInUser={loggedInUser}>
        <UserProfileList />
    </AuthorizedRoute>
    }
/>
```

Add this function to the `userProfileManager`:
``` javascript
export const getUserProfilesWithRoles = () => {
  return fetch(_apiUrl + "/withroles").then((res) => res.json());
};
```

Add this endpoint to the `UserProfileController`:
``` csharp
[HttpGet("withroles")]
[Authorize(Roles = "Admin")]
public IActionResult GetWithRoles()
{
    return Ok(_dbContext.UserProfiles
    .Include(up => up.IdentityUser)
    .Select(up => new UserProfile
    {
        Id = up.Id,
        FirstName = up.FirstName,
        LastName = up.LastName,
        Address = up.Address,
        Email = up.IdentityUser.Email,
        UserName = up.IdentityUser.UserName,
        IdentityUserId = up.IdentityUserId,
        Roles = _dbContext.UserRoles
        .Where(ur => ur.UserId == up.IdentityUserId)
        .Select(ur => _dbContext.Roles.SingleOrDefault(r => r.Id == ur.RoleId).Name)
        .ToList()
    }));
}
```
- This is a very inefficient way to do this query, but better solutions require a level of complexity that is not necessary right now. The query gets user profiles, then searches for user roles associated with the profile, and maps each of those to role names.

The `promote` and `demote` functions currently only log to the console, but the component should be viewable now. Test it to see if admins have a demote button, and non-admins have a promote button (you might have to register a few more users). Use the console to check that the buttons work.

## Adding Promote and Demote Endpoints

Add the following endpoints to `UserProfileController`:
``` csharp
[HttpPost("promote/{id}")]
[Authorize(Roles = "Admin")]
public IActionResult Promote(string id)
{
    IdentityRole role = _dbContext.Roles.SingleOrDefault(r => r.Name == "Admin");
    // This will create a new row in the many-to-many UserRoles table.
    _dbContext.UserRoles.Add(new IdentityUserRole<string>
    {
        RoleId = role.Id,
        UserId = id
    });
    _dbContext.SaveChanges();
    return NoContent();
}

[HttpPost("demote/{id}")]
[Authorize(Roles = "Admin")]
public IActionResult Demote(string id)
{
    IdentityRole role = _dbContext.Roles
        .SingleOrDefault(r => r.Name == "Admin"); 
    IdentityUserRole<string> userRole = _dbContext
        .UserRoles
        .SingleOrDefault(ur =>
            ur.RoleId == role.Id &&
            ur.UserId == id);

    _dbContext.UserRoles.Remove(userRole);
    _dbContext.SaveChanges();
    return NoContent();
}
```

Add these functions to `userProfileManager`:
``` javascript
export const promoteUser = (userId) => {
  return fetch(`${_apiUrl}/promote/${userId}`, {
    method: "POST",
  });
};

export const demoteUser = (userId) => {
  return fetch(`${_apiUrl}/demote/${userId}`, {
    method: "POST",
  });
};
```
Finally, update the functions in `UserProfileList` to use the data access functions from `userProfileManager`:
>UserProfileList.jsx
``` javascript
 const promote = (id) => {
    promoteUser(id).then(() => {
      getUserProfilesWithRoles().then(setUserProfiles);
    });
  };
  const demote = (id) => {
    demoteUser(id).then(() => {
      getUserProfilesWithRoles().then(setUserProfiles);
    });
  };
```
You should now be able to test the promote and demote functionality for users. Promotion and Demotion is only allowed by admins (because `Roles = "Admin"`) is passed into the `Authorize` attribute on the controller methods. The route should only even be visible to admins because of the `roles={["Admin"]}` prop passed to `AuthorizedRoute` for the route. 

Sign in as a non-admin to see that an attempt to navigate to `/employees` results in a redirect to the `Login` component. `Employees` shouldnot appear on the navbar for no-admins. 

The code in this chapter is complex, and you only are responsible for knowing how to use the `Authorize` attribute to protect an endpoint based on roles.




    
