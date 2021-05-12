# NClient
NClient is an automatic type-safe .Net HTTP client that allows you to call web service API methods using annotated interfaces or controllers. The client supports asynchronous calls, http contexts, retry policies, and logging. The main difference between NClient and its analogues is that NClient allows you to annotate ASP.NET controllers via interfaces and then use these interfaces to create clients. Annotated interfaces allow you to get rid of unwanted dependencies on a client side and to reuse an API description in clients without boilerplate code.

## Why use NClient?
Creating clients for web services can be quite a challenge because, in addition to data transfer, you need to implement query building, serialization, retry policy, error handling, logging — and this is not to mention the maintenance that comes with each update of your APIs. What if you could create clients with a fraction of the effort? This is exactly what NClient hopes to achieve by allowing you to create clients declaratively.

## How to install?
The easiest way is to install [NClient package](https://www.nuget.org/packages?q=Tags%3A"NClient") using Nuget. How to choose which package you need, see below in "NuGet packages" section.
