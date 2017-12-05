---
title: Claim based Authorization in Asp.Net Core 
author: Wijitha Wijenayake 
linkedin: https://lk.linkedin.com/in/wijitha-wijenayake-63813b53/de
---

Authentication and Authorization are two most essential features in any
kind of application. In Microsoft .Net Technology Stack, Identity
Framework is the tool designed to achieve this.

In this post I’m not going to talk about the Authentication. I will only
show you how we can implement a fully customized Authorization Engine
for an Asp.Net Web Application using some of the cool features of
Identity Framework.

Before proceed, I assume that you have some understanding on Asp.Net
Core, Identity Framework, Token and Cookie based Authentication and MVC
architecture.

**The Easy and Common Way**

The most common and simplest approach is to implement Role based
authorization by decorating Controllers and Actions with Authorize
attribute with the allowed Role(s).

```csharp
[Authorize(Role=”Manager”)]
public class IndexController
{

}
```
**Problem with This Approach**

This approach will work fine for small applications as long as users do
not want complex functionalities related to permissions. However if you
have to or will have to answer ‘Yes’ to any of the following questions
you might be in trouble down the line.

1.  Would new Roles needs to be added to the system?

2.  Add permissions to the new Roles?

3.  Change the permissions of existing roles? (Ex: Remove user deletion
    permission from Manager Role)

4.  Implement an admin UI where users can manage Roles and Permissions
    in the system

As in Role based authorization, if the permissions are hard coded based
on roles, it will be challenging if there are any changes related to
permissions. Most likely you will have to modify the existing working
code, resulting having to perform regression testing each and every time
which would take significant amount of time.

**Claim based Authorization with Identity Core**

The proposed solution is to implement a fully customized Authorization
Engine using Identity Framework. Initially it might look bit complex.
But once the initial ground work is done, the rest of the work will be
smooth. I will try to keep things simple as much as possible.

**Step 01**

Define a custom claim type as “permission”. Claim types are just
strings. Let’s create a const class to hold the values for consistency.

```csharp
public class CustomClaimTypes
{
    public const string Permission = "projectname/permission";
}
```

**Step 02**

Next, we need to identify the areas of the system based on business
logic and define permissions for those areas. Again these are strings
and, I’m creating a const class to hold these values. Let’s assume our
system has Users and Teams management areas.

```csharp
public static class Users
{
    public const string Add = "users.add";
    public const string Edit = "users.edit";
    public const string EditRole = "users.edit.role";
}

public static class Teams
{
    public const string AddRemove = "teams.addremove";
    public const string EditManagers = "teams.edit.managers";
    public const string Delete = "teams.delete";
}
```

**Step 03**

Next step is assigning permission claims to each role based on the
requirement. This can be done in various ways. For simplicity I create
the initial roles and assigned their permission claims in Entity
Framework Database initializer method (Seed method).

```csharp
await roleManager.CreateAsync(new ApplicationRole("Manager"));

await roleManager.CreateAsync(new ApplicationRole("User"));

var userRole = await roleManager.FindByNameAsync("User");

await roleManager.AddClaimAsync(userRole, new Claim(CustomClaimTypes.Permission, Permissions.User.View));

await roleManager.AddClaimAsync(userRole, new Claim(CustomClaimTypes.Permission, Permissions.Team.View));

var managerRole = await roleManager.FindByNameAsync("Manager");

await roleManager.AddClaimAsync(managerRole, new Claim(CustomClaimTypes.Permission, Permissions.Users.Add));

await roleManager.AddClaimAsync(managerRole, new Claim(CustomClaimTypes.Permission, Permissions.Teams.Addremove));
```

Role claims are saved in AspNetRoleClaims table. If you prefer you can
do this purely in database level. Or else you can give a UI to configure
this from the front end. But that might take more time and if there is
no specific customer requirement, it will be over an over kill.

**Step 04**

As the next step, we need to add these claims to authentication token or
cookie (Based on the type of authentication used). I’m not sure in
cookie authentication whether the role claims are added the to the Auth
cookie by default. If not you have to add them manually. In the token
based approach you should add them manually. Following is how you do it
in token based model.

```csharp
var roles = await _userManager.GetRolesAsync(user);

var userRoles = roles.Select(r => new Claim(ClaimTypes.Role, r)).ToArray();

var userClaims = await _userManager.GetClaimsAsync(user).ConfigureAwait(false);
var roleClaims = await GetRoleClaimsAsync(roles).ConfigureAwait(false);

var claims = new[]
             {
                 new Claim(ClaimTypes.NameIdentifier, user.Id.ToString()),
                 new Claim(ClaimTypes.Email, user.Email),
                 new Claim(ClaimTypes.Name, user.UserName)
             }.Union(userClaims).Union(roleClaims).Union(userRoles);

var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_jwtSettings.SigningKey));
var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

var token = new JwtSecurityToken(
    issuer: _jwtSettings.Issuer,
    audience: _jwtSettings.Audience,
    claims: claims,
    expires: DateTime.UtcNow.AddYears(1),
    signingCredentials: creds);

```

By now we have defined a custom claim type, added various claim values
(permissions) against each role based on permissions and injected role
claims to Auth token/cookie. We are almost there, just couple of steps
left.

**Step 05**

Next step is to define Authorization Policies based on the claims. Again
we are creating a const class to hold the policy names.

```csharp
public static class PolicyTypes
{
    public static class Users
    {
        public const string Manage = "users.manage.policy";
        public const string EditRole = "users.edit.role.policy";
    }

    public static class Teams
    {
        public const string Manage = "teams.manage.policy";

        public const string AddRemove = "teams.addremove.policy";
    }
```

After that we need to setup the policies. In Asp.Net Core we will do
this in “ConfigureServices()” in the Startup.cs. And this logic should
be placed after configuring Identity.

```csharp
services.AddAuthorization(options =>
{
options.AddPolicy(PolicyTypes.Teams.Manage, policy => { policy.RequireClaim(CustomClaimTypes.Permission, Permissions.Teams.Manage); });
            options.AddPolicy(PolicyTypes.Teams.AddRemove, policy => { policy.RequireClaim(CustomClaimTypes.Permission, Permissions.Teams.AddRemove); });
options.AddPolicy(PolicyTypes.Users.Manage, policy => { policy.RequireClaim(CustomClaimTypes.Permission, Permissions.Users.Add); });
            options.AddPolicy(PolicyTypes.Users.EditRole, policy => { policy.RequireClaim(CustomClaimTypes.Permission, Permissions.Users.EditRole); });
}

```

**Step 06**

By now we have got all we need. Once you have the policies configured,
next step is to decorate the Controllers and Actions with Authorize
attribute along with policies.

```csharp
[Authorize(Policy = PolicyTypes.Teams.Manage)]
public async Task<IEnumerable<TeamDto>> GetSubTeams(int parentId)
{
    var teams = await _teamService.GetSubTeamsAsync(parentId);
    return teams;
 }

```

**Key Benefits of this Approach**

You can achieve following things without modifying the code. Even if
there is no UI developed, configuration can be easily done in database
level.

-   Can alter permission for Roles easily

-   New Roles can be added and configure the permissions any time

It may looks like lots of work. But once the framework is setup at the
beginning of a project, rest of the development and maintenance will be
lot easier.
