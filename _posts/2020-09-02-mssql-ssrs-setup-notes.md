---
layout: post
title: MSSQL SSRS setup notes
date: 2020-09-02 17:31:10
author: Peter Stevenson
summary: Setup notes for SQL Server Reporting Services
categories: sysadmin
thumbnail:
tags:
 - Windows
 - SQL
---

# MSSQL SSRS setup notes

## Debugging

If you encounter issues during setup you may want to enable debugging to get better error messages.

Use SSMS (SQL Management Studio) to connect to the reporting server. Ensure the "Server type:" is set to "Reporting Services".

Right click on the report server.

Enable "remote errors" under properties.

## Logins and security

### Service account

The actual service runs under: "NT Service\ReportServer" below we are discussing creating a user for our application to talk to SSRS.

### Connections to SSRS

Create local or domain user account. This can then be used by your software in a connection string to make connections to the reporting service over HTTP/HTTPS.

In a browser on the SSRS server navigate to [http://localhost/Reports](http://localhost/Reports) and assign the user to "System User" role under settings cog top right "Site settings"/"Security".

Going back to original home page right click a reports folder or use "Manager folder" on the black ribbon bar (to apply to all reports folders) and assign the user correct permissions for those reports. E.g. "Browser"

### Data source

If your SSRS data source is set to log into the data source: "As the user viewing the report" then you will need to add the new user account we created to the SQL server logins.

Map this login to each DB the reporting service will be retrieving data from.

Add execution permissions to your user if reporting service will call store procedures:

```sql
USE [MyDb]
GO
GRANT EXECUTE TO [MyUser]
```

_You can also do this on a per SP level for more security._

Alternatively you can supply a different set of credentials SSRS will use to talk to the SQL data source.