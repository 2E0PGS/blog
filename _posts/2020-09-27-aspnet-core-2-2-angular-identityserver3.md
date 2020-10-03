---
layout: post
title: ASP.NET Core 2.2/Angular 7 auth IdentityServer3
date: 2020-09-27 12:09:19
author: Peter Stevenson
summary: Auth against IdentityServer3 in ASP.NET Core 2.2 and Angular 7
categories: programming
thumbnail:
tags:
 - API
 - Core
 - NuGet
 - Angular
 - JWT
 - IdentityServer
 - .NET
 - ASP.NET
 - C#
---

# Auth against IdentityServer3 in ASP.NET Core 2.2 and Angular 7

## ASP.NET Core 2.2 Web API

Using `IdentityServer4.AccessTokenValidation` NuGet packages to auth against IdentityServer3.

### Startup.cs

```
services.AddAuthentication(IdentityServerAuthenticationDefaults.AuthenticationScheme)
.AddJwtBearer(
options =>
{
    options.Authority = "https://youridentityserver3instance.com/identity";
    //options.RequireHttpsMetadata = false; // If you don't use HTTPS uncomment.
    options.TokenValidationParameters = new Microsoft.IdentityModel.Tokens.TokenValidationParameters();
    //options.TokenValidationParameters.ValidateAudience = false; // Try uncomment this if it's not working.
    options.SaveToken = true; // Allows the API to reuse it's token later in client calls.

    // If you need to fix claim mapping to be backwards compatible.
    options.SecurityTokenValidators.Clear();
    options.SecurityTokenValidators.Add(new JwtSecurityTokenHandler
    {
        InboundClaimTypeMap = new Dictionary<string, string>
        {
            { "name",System.Security.Claims.ClaimTypes.Name },
            { "role", System.Security.Claims.ClaimTypes.Role }
        }
    });
});
```

## ASP.NET Core 2.2 MVC

Using `IdentityServer4.AccessTokenValidation` NuGet packages to auth against IdentityServer3.

### Startup.cs

```
// If you need to fix claim mapping to be backwards compatible.
JwtSecurityTokenHandler.DefaultInboundClaimTypeMap = new Dictionary<string, string>
{
    { "name",System.Security.Claims.ClaimTypes.Name },
    { "role", System.Security.Claims.ClaimTypes.Role }
};

// Add cookie authentication.
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(
    options =>
    {
        options.ExpireTimeSpan = new TimeSpan(0, 30, 0);
        options.SlidingExpiration = true;
    });

// Add OpenId authentication.
services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
    .AddOpenIdConnect(
    options =>
    {
        options.Authority = "https://youridentityserver3instance.com/identity";
        options.ClientId = "myclient";
        options.ClientSecret = "mysecret";
        options.SignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.ResponseType = "code id_token token";
        options.UseTokenLifetime = false;
        options.SignedOutRedirectUri = "http://localhost:2000/";

        options.SaveTokens = true; // Allows the MVC app to reuse it's token later in client calls.
        options.GetClaimsFromUserInfoEndpoint = true;
        options.Scope.Add("openid");
        options.Scope.Add("profile");
        options.Scope.Add("address");
        options.Scope.Add("offline_access");
        options.Scope.Add("roles");
    });
```

## Angular 7

Angular can use [IdentityModel/oidc-client-js](https://github.com/IdentityModel/oidc-client-js) to auth against IdentityServer3.

### environment.ts

```
  openIdConnectSettings: {
    authority: 'https://youridentityserver3instance.com/identity',
    client_id: 'myclient',
    redirect_uri: 'http://localhost:1000/signin-oidc',
    post_logout_redirect_uri: "http://localhost:1000/",
    scope: 'openid profile',
    response_type: 'id_token token',
    filterProtocolClaims: true,
    loadUserInfo: true,
    revokeAccessTokenOnSignout: true,
    monitorSession: true,
    automaticSilentRenew: true,
    silent_redirect_uri: 'http://localhost:1000/redirect-silentrenew'
  },
```
