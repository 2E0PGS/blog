---
layout: post
title: ASP.NET WebHooks
date: 2020-07-25 14:29:19
author: Peter Stevenson
summary: WebHooks in ASP.NET
categories: programming
thumbnail:
tags:
 - WebHooks
 - Web
 - Framework
 - API
 - Core
 - ASP.NET
---

# WebHooks in ASP.NET

I mostly discuss code for ASP.NET Framework but most if not all is interchangeable with ASP.NET Core.

The following official blog post document most of the setting up:

[Sending WebHooks with ASP.NET WebHooks and custom filters](https://devblogs.microsoft.com/aspnet/sending-webhooks-with-asp-net-webhooks-preview/)

[Storing WebHook Registrations in SQL](https://devblogs.microsoft.com/aspnet/updates-to-microsoft-asp-net-webhooks-preview/)

## NotifyAllAsync

`NotifyAsync` just notifies same user. `NotifyAllAsync` notifies all users. Both can have further filtering via predicate, see: [new-year-updates-to-asp-net-webhooks-preview](https://devblogs.microsoft.com/aspnet/new-year-updates-to-asp-net-webhooks-preview/)

## noecho?

Without this param on the register request the `WebHookUri` will need to respond to an echo request to prove its alive. Append `noecho?` to the end of your `WebHookUri` to prevent this behaviour.

For example: `http://localhost:59927/api/webhooks/incoming/custom?noecho`

Not sure if actually documented I think I reverse engineered this from the source code, see: [WebHookManager.cs#L201](https://github.com/aspnet/AspNetWebHooks/blob/7a7554b2748cc094ff1fa151de3e1b1b56d32511/src/Microsoft.AspNet.WebHooks.Custom/WebHooks/WebHookManager.cs#L201)

## Subscriber identifier/userid resolution

[ApiControllerExtensions.cs#L88](https://github.com/aspnet/AspNetWebHooks/blob/7a7554b2748cc094ff1fa151de3e1b1b56d32511/src/Microsoft.AspNet.WebHooks.Custom/Extensions/ApiControllerExtensions.cs#L88)

[WebHookUser.cs#L43](https://github.com/aspnet/AspNetWebHooks/blob/7a7554b2748cc094ff1fa151de3e1b1b56d32511/src/Microsoft.AspNet.WebHooks.Custom/WebHooks/WebHookUser.cs#L43)

### WebHookUser.IdClaimsType

Initializes and overrides this: [WebHookUser.cs#L26](https://github.com/aspnet/AspNetWebHooks/blob/7a7554b2748cc094ff1fa151de3e1b1b56d32511/src/Microsoft.AspNet.WebHooks.Custom/WebHooks/WebHookUser.cs#L26)

See below example in `Startup.cs`

## Startup.cs

```
// Initialize MS WebHook Apis.
config.InitializeCustomWebHooks();
config.InitializeCustomWebHooksSqlStorage();
config.InitializeCustomWebHooksApis();

// Override the default subscriber identifier key.
// This goes into the "WebHooks" table under the "User" column.
WebHookUser.IdClaimsType = CustomClaimTypes.MyCustomId;
```

## Duplicate registrations

Duplicate WebHook registrations for same user: [aspnet/WebHooks/issues/100](https://github.com/aspnet/WebHooks/issues/100)

I ended up creating a custom check for this.

You can disable encryption on the table for easier debugging using: `config.InitializeCustomWebHooksSqlStorage(false);`