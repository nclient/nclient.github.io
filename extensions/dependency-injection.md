# Dependency injection
You can create clients using `Microsoft.DependencyInjection`. If you want to use a controller-based client, 
the extension method `AddNClient` is already available to you. If you need an interface-based client, 
then install [NClient.Extensions.DependencyInjection](https://www.nuget.org/packages/NClient.Extensions.DependencyInjection) package 
that contains `AddNClient` extension methods.

```ruby
var serviceProvider = new ServiceCollection()
    .AddLogging()
    .AddNClient<IProductServiceClient>(host: "http://localhost:8080")
    .BuildServiceProvider();

IProductServiceClient client = serviceProvider.GetRequiredService<IProductServiceClient>();
```
