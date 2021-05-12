# FAQ

## I don't want to use RestSharp and Polly, what should I do?
You should write your own providers and use them. How to do this, see [here](/providers/index.md).

## Can I use NClient for services that are not written in C#?
Yes, you can. See [this example](/examples/non-dot-net-service.md).

## I need authentication, how do I specify it when creating a client?
You should pass `HttpClientHandler` instance 
(see Microsoft [documentation](https://docs.microsoft.com/en-us/dotnet/api/system.net.http.httpclienthandler)) 
to `HttpClient`, and then use the provider instance to return the client (method `SetHttpClientProvider`).

## Why do I need resilience?
This is well written in [the article](https://docs.microsoft.com/en-us/dotnet/architecture/cloud-native/application-resiliency-patterns).

## How do I enable logs?
To enable logging, you should pass the `ILogger<>` instance to `ClientProvider` (method `WithLogger`). 
For more information, see [here](/features/logging.md).

## How do I add the client to IServiceCollection?
You need to install `NClient.Extensions.DependencyInjection`, after that, the `AddNClient`extension method will be available to you.

## How do I get a full response from the service?
Use methods of `IHttpNClient` interface. For more information, see [here](/features/http-response.md).

## Can I use different resilience settings for one method?
Yes, use methods of `IResilienceNClient` interface. For more information, see [here](/features/resilience.md).

## How do I pass xml to the request body instead of json?
To do this, you should write your implementation of `IHttpClientProvider`. 
For more information, see [here](/providers/http-client-provider.md).

## How do I make sure that the interface for the client is annotated correctly?
The client is validated when its instance is created using `NClientProvider`. If the interface is incorrect, an exception will be thrown.

## Is the client thread-safe?
Yes, client instances are thread-safe.

## NClient is compatible with which versions .Net?
NClient is compatible with all versions that support .NET Standard 2.0 and .NET Standard 2.1. 
The compatibility table can be viewed [here](https://docs.microsoft.com/en-us/dotnet/standard/net-standard).

## What versions ASP.NET supported?
NClient supports only ASP.NET Core.

## I have another question, where do I ask it?
You can create a [issue](https://github.com/nclient/NClient/issues) or ask a question in [discussions](https://github.com/nclient/NClient/discussions).
