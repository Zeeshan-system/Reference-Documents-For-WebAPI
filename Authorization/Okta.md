# Okta for Auhentication and Authorization

## Setting up Okta Developer Workspace
* Login to [Okta Developer](https://developer.okta.com/) and sign up using Google Credentials.
* Create an App Integration on Applications menu with Sign-in method OIDC and Application type Web Application.
* Grant type Client Credentials. 
* Sign-in redirect URIs: {hostname:Port_of_application}/okta-auth
* Setup Assignment Groups: Everyone and your_desired_Group
* (Ad-Hoc) Groups can be created in Directory>Groups.
* (Ad-Hoc) People need to be registered in Directory>People in order to add in Groups or get access to application (Everyone group).
* Copy the ClientID, Secret and the Callback path (Sign-in redirect URIs).

## Installations
### Setting up Program.cs
* Added Okta Cookie
```csharp
builder.Services.AddAuthentication(options =>
  {
    options.DefaultScheme=CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme="okta";
  })
  .AddCookie(options=>{
    options.LoginPath="/login";
    options.AccessDeniedPath="/denied";
  })
  .AddOpenIdConnect("okta",options=>
  {
    options.Authority=builder.Configuration["Okta:Authority"];
    options.ClientId=builder.Configuration["Okta:ClientID"];
    options.ClientSecret=builder.Configuration["Okta:ClientSecret"];
    options.CallbackPath=builder.Configuration["Okta:CallBackPath"];
    options.ResponseType="code";
    options.SaveTokens=true;
    options.Scope.Add("profile");
  });

builder.Services.AddAuthorization();
```
## Setting up appsettings.json
```json
"Okta":{
        "Authority":"{Your Okta Developer URL}",
        "ClientID":"{Your Client ID}",
        "ClientSecret":"Your Client Secret",
        "CallBackPath":"/okta-auth"
    }
```
## Insert Authorize tag above Controllers to Authorize
```csharp
[Authorize]
```

## Noted Points
* Okta Authentiication only works under HTTPS. If SSL s not setup then it won't redirect to OKTA sign-in page.