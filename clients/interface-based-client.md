# Interface-based client
NClient allows you to create clients not only for ASP.NET, but also non ASP.NET services. 
To do this, you need to create an interface and additionally declare it with attributes from `NClient.InterfaceProxy.Attributes`:

```ruby
[Api(template: "api")]
public interface IProductServiceClient : INClient
{
    [AsHttpPost(template: "products")]
    Task PostAsync([ToBody] Product product);
}
```

> :exclamation: Please note that the interface must inherit from `INClient` interface.  

Now that you have an interface for the client, you can create a client:
```ruby
IProductServiceClient client = new AspNetClientProvider()
    .Use<IProductServiceClient >(host: new Uri("http://localhost:8080"))
    .SetDefaultHttpClientProvider()
    .WithoutResiliencePolicy()
    .Build();
```

### NClient.InterfaceProxy.Attributes
They are very similar to attributes for ASP.NET controllers. Below there is a table with the existing attributes and their equivalents in ASP.NET.

| NClient.InterfaceProxy.Attributes | Microsoft.AspNetCore.Mvc |
|:----------------------------------|:-------------------------|
| ApiAttribute | ApiControllerAttribute + RouteAttribute |
| AsHttpGetAttribute | HttpGetAttribute |
| AsHttpPostAttribute | HttpPostAttribute |
| AsHttpPutAttribute | HttpPutAttribute |
| AsHttpDeleteAttribute | HttpDeleteAttribute |
| ToQueryAttribute | FromQueryAttribute |
| ToBodyAttribute | FromBodyAttribute |
| ToRouteAttribute | FromRouteAttribute |
| ToHeaderAttribute | FromHeaderAttribute |

### Limitations
NClient attributes have some limitations:

#### Multiple attributes:
```ruby
[AsHttpGet("weather"), AsHttpGet("weather/{date}")] // => NotSupportedNClientException
WeatherForecast GetAsync(DateTime date);
```

#### Multiple body parameters:
```ruby
[AsHttpPost]
void Post([ToBody] WeatherForecast forecast, [ToBody] DateTime date);
```
Exception `NotSupportedNClientException` will be thrown.

#### Non standard types in route template:
```ruby
[AsHttpPost("weather/{forecast}")]
void PostAsync([ToBody] WeatherForecast forecast);
```
Exception `NotSupportedNClientException` will be thrown.

#### Non standard types in headers:
```ruby
[AsHttpPost]
void Post([ToHeader] WeatherForecast forecast);
```
Exception `NotSupportedNClientException` will be thrown.

#### Dictionaries of non standard types in query:
```ruby
[AsHttpGet]
void Get([ToQuery] Dictionary<int, WeatherForecast> forecasts);
```
Exception `NotSupportedNClientException` will be thrown.

#### Arrays of non standard types in query:
```ruby
[AsHttpGet]
void Get([ToQuery] WeatherForecast[] forecasts);
```
Exception `NotSupportedNClientException` will be thrown.
