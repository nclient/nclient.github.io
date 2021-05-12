# NuGet Packages

| Package name                                             | Description                                            | Dependencies                                           |
| :------------------------------------------------------- | :----------------------------------------------------- |:------------------------------------------------------ |
| [NClient](https://www.nuget.org/packages/NClient) | Tools for creating clients from interfaces and controllers including third-party | Castle, System.Text.Json, System.Net.Http, Polly |
| [NClient.Standalone](https://www.nuget.org/packages/NClient.Standalone) | The same as NClient package, but without third-party | Castle |
| [NClient.AspNetCore](https://www.nuget.org/packages/NClient.AspNetCore) | Allows you to annotate controllers via interfaces | Castle, Microsoft.AspNetCore.Mvc.* (Core, ApiExplorer, Cors, DataAnnotations) |
| [NClient.Extensions.DependencyInjection](https://www.nuget.org/packages/NClient.Extensions.DependencyInjection) | Extension methods for registration of clients in ServiceCollection | Castle, Microsoft.Extensions.DependencyInjection, System.Text.Json, System.Net.Http, Polly |
| [NClient.Abstractions](https://www.nuget.org/packages/NClient.Abstractions) | Abstractions for clients and providers | - |
| [NClient.Annotations](https://www.nuget.org/packages/NClient.Annotations) | Attributes for annotation of client interfaces and controllers | - |
| [NClient.Providers.Resilience](https://www.nuget.org/packages/NClient.Providers.Resilience) | Polly based resilience policy provider | Polly |
| [NClient.Providers.HttpClient.System](https://www.nuget.org/packages/NClient.Providers.HttpClient.System) | System.Net.Http.HttpClient based HTTP client provider | System.Text.Json, System.Net.Http |
| [NClient.Providers.HttpClient.RestSharp](https://www.nuget.org/packages/NClient.Providers.HttpClient.RestSharp) | RestSharp based HTTP client provider | System.Text.Json, RestSharp |
