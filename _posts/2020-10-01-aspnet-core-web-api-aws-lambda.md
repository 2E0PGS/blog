---
layout: post
title: ASP.NET Core Web API AWS Lambda
date: 2020-10-01 19:33:19
author: Peter Stevenson
summary: Testing AWS Lambda with ASP.NET Core Web API
categories: programming
thumbnail:
tags:
 - API
 - Core
 - AWS
 - .NET
 - ASP.NET
 - C#
---

# ASP.NET Core Web API AWS Lambda

I am using the below two tools.

* [AWS Lambda for .NET Core](https://github.com/aws/aws-lambda-dotnet/)
* [.NET Core CLI](https://docs.aws.amazon.com/lambda/latest/dg/csharp-package-cli.html)

## Blueprint types

Blueprint and template are interchangeable words here.

### serverless.AspNetCoreWebAPI

`dotnet new serverless.AspNetCoreWebAPI`

Removed the S3Controller and related config vars and injector.

This assumes you have `.aws\credentials` setup in home directory.

Edit `aws-lambda-tools-defaults.json` update `profile` and `region` fields. Otherwise i got `Error retrieving configuration for function The operation was canceled` when deploying.

Here you can use attributes.

#### Related videos

* [Create a Serverless .NET Core 2.1 Web API with AWS Lambda](https://youtu.be/OhEANj3Y6ZQ)
* [Create & Deploy Serverless Dot Net Core Web API with Lambda Fucntion & API Gateway](https://youtu.be/HVW-g-5w_gg)

#### Related blog posts

* [Creating a Serverless Application with ASP.NET Core, AWS Lambda and AWS API Gateway](https://www.jerriepelser.com/blog/aspnet-core-aws-lambda-serverless-application/)

So its not just me who removes the extra bits or creates the API Gateway manually.

Blueprint method signature:

```
[HttpGet("{id}")]
public string Get(int id)
```

`public void`, `public string` or `public IActionResult` all work.

#### Modified example

Based on ASP.NET Core 3.1

[bitbucket.org/2E0PGS/aws-serverless-aspnet-core-api](https://bitbucket.org/2E0PGS/aws-serverless-aspnet-core-api)

#### cURL example

##### Request

```
curl --request POST \
  --url https://redacted.execute-api.eu-west-1.amazonaws.com/default/v1/values \
  --header 'content-type: application/json' \
  --header 'x-api-key: redacted' \
  --data '{
	"data": "testing"
}'
```

##### Response

```
{
    "motd": "Hello from AWS serverless.",
    "data": "testing",
    "environmentMachineName": "169",
    "environmentDirectory": "/var/task",
    "environmentSystemDirectory": "",
    "currentCulture": "English (United States)",
    "currentDateTimeUtc": "2020-09-30T17:19:50.8216401Z",
    "method": "POST"
}
```

#### Insomnia example

![aspnet-core-web-api-insomnia](/blog/assets/2020-10-01/aspnet-core-web-api-insomnia.png)

### lambda.EmptyFunction

`dotnet new lambda.EmptyFunction` 

Edit `aws-lambda-tools-defaults.json` update `profile` and `region` fields. Already comes with a `function-handler` JSON field.

Works but isn't designed for API Gateway input really. Just basic string input.

Blueprint method signature: `public string FunctionHandler(RequestModel requestModel, ILambdaContext context)`

#### Example

```
public string FunctionHandler(RequestModel requestModel, ILambdaContext context)
{
    return requestModel.lowercase?.ToUpper();
}
```

### serverless.EmptyServerless

`dotnet new serverless.EmptyServerless` 

Edit `aws-lambda-tools-defaults.json` update `profile` and `region` fields.

Blueprint method signature: `public APIGatewayProxyResponse Get(APIGatewayProxyRequest request, ILambdaContext context)`

I would have to discern the request type based on `request.HttpMethod`. I am not sure if you can use the ASP attributes here.

#### Modified example

```
public APIGatewayProxyResponse Post(APIGatewayProxyRequest request, ILambdaContext context)
{
    context.Logger.LogLine("Post Request\n");

    var response = new APIGatewayProxyResponse
    {
        StatusCode = (int)HttpStatusCode.OK,
        Body = "Hello from AWS Serverless." +
        "\n This is a 2E0PGS test" +
        "\n Request body: " + request.Body + 
        "\n Environment directory: " + Environment.CurrentDirectory + 
        "\n Environment machinename: " + Environment.MachineName + 
        "\n Environment version: " + Environment.Version +
        "\n Datetime: " + DateTime.Now.ToLongDateString() + 
        "\n Culture: " + CultureInfo.CurrentCulture.DisplayName + 
        "\n Method: " + request.HttpMethod,
        Headers = new Dictionary<string, string> { { "Content-Type", "text/plain" } }
    };

    return response;
}
```

#### Insomnia example

![empty-serverless-insomnia](/blog/assets/2020-10-01/empty-serverless-insomnia.png)

## Deploy types

### deploy-function

Deploys just the Lambda function itself. I think this is much simpler and doesn't require a S3 bucket. 

However you need to carefully configure the API Gateway from default to work with an existing Lambda Function with a API Gateway trigger created using the AWS console (web UI) as I describe what will happen otherwise below.

![add-lambda-api-gateway-trigger](/blog/assets/2020-10-01/add-lambda-api-gateway-trigger.png)

![aws-lambda-layout](/blog/assets/2020-10-01/aws-lambda-layout.png)

I have to hack the `aws-lambda-tools-defaults.json` to include a `function-handler` and a `function-runtime` based on `serverless.template` `AwsServerlessAspNetCore.Api::AwsServerlessAspNetCore.Api.LambdaEntryPoint::FunctionHandlerAsync`

You have to specify runtime. `dotnetcore3.1` unless you define it in `aws-lambda-tools-defaults.json` under property: `function-runtime`

They probably prefer you using the `serverless.template` because it adds the method routing into API Gateway automatically. Otherwise you may run into the below.

#### Default API Gateway routes

![default-api-gateway-routes](/blog/assets/2020-10-01/default-api-gateway-routes.png)

Doesn't work, returns 405 method not allowed.

```
[HttpPost]
public string Post([FromBody]string value)
```

The below do work, returns 200. This appears to be a routing issue.

```
[HttpPost("{id}")]
public string Post([FromBody]string value)
```

```
[HttpPost("{anything}")]
public string Post([FromBody]string value)
```

```
[HttpPost]
[Route("{anything}")]
public string Post([FromBody]string value)
```

It will return unsupported media type for POST unless you send with header `Content-Type` `application/json`

#### Using API Gateway proxy+ route

![api-gateway-resources-api-key](/blog/assets/2020-10-01/api-gateway-resources-api-key.png)

Working routing on this without ASP "route template" if using `{proxy+}`

```
    [Route("v1/values")]
    public class ValuesController : ControllerBase
    {
        [HttpPost]
        public string Post([FromBody]string value)
```

Manually add auth via API key: [Set up API keys using the API Gateway console](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-setup-api-key-with-console.html)

Repeat after me, I will not forget to deploy my API Gateway: [AWS API Gateway: Solving Missing Authentication Tokens](http://www.awslessons.com/2017/aws-api-gateway-missing-authentication-token/)

![api-gateway-deploy-1](/blog/assets/2020-10-01/api-gateway-deploy-1.png)

![api-gateway-deploy-2](/blog/assets/2020-10-01/api-gateway-deploy-2.png)

![api-gateway-stages](/blog/assets/2020-10-01/api-gateway-stages.png)

Run: `dotnet lambda deploy-function`

### deploy-serverless

Edit: `aws-lambda-tools-defaults.json` update `profile` and `region` fields.

Edit: `serverless.template`

Run: `dotnet lambda deploy-serverless --region eu-west-1`

It uses AWS CloudFormation to create a S3 bucket to store the code and setup a AWS API Gateway with `{proxy+}` routes.
