# Interface-based client
NClient allows you to create clients not only for ASP.NET, but also any services. 
To do this, you need to create an interface and additionally declare it with attributes from `NClient.Annotations`:

```ruby
[Path("api")]
public interface IProductServiceClient : INClient
{
    [PostMethod("products")]
    Task PostAsync([BodyParam] Product product);
}
```

> :exclamation: Please note that the interface must inherit from `INClient` interface.  

Now that you have an interface for the client, you can create a client:
```ruby
IProductServiceClient client = NClientProvider
    .Use<IProductServiceClient>(host: "http://localhost:8080")
    .Build();
```

### NClient.Annotations
They are very similar to attributes for ASP.NET controllers. Below there is a table with the existing attributes and their equivalents in ASP.NET.

| NClient.InterfaceProxy.Attributes | Microsoft.AspNetCore.Mvc |
|:----------------------------------|:-------------------------|
| PathAttribute | RouteAttribute |
| GetMethodAttribute | HttpGetAttribute |
| PostMethodAttribute | HttpPostAttribute |
| PutMethodAttribute | HttpPutAttribute |
| DeleteMethodAttribute | HttpDeleteAttribute |
| QueryParamAttribute | FromQueryAttribute |
| BodyParamAttribute | FromBodyAttribute |
| RouteParamAttribute | FromRouteAttribute |
| HeaderParamAttribute | FromHeaderAttribute |

### Limitations
NClient attributes have some limitations:

#### Multiple attributes:
```ruby
[GetMethod("weather"), GetMethod("weather/{date}")] // => NotSupportedNClientException
WeatherForecast GetAsync(DateTime date);
```

#### Multiple body parameters:
```ruby
[PostMethod]
void Post([BodyParam] WeatherForecast forecast, [BodyParam] DateTime date);
```
Exception `NotSupportedNClientException` will be thrown.

#### Non primitive types in route template:
```ruby
[PostMethod("weather/{forecast}")]
void PostAsync([BodyParam] WeatherForecast forecast);
```
Exception `NotSupportedNClientException` will be thrown.

#### Non primitive types in headers:
```ruby
[PostMethod]
void Post([HeaderParam] WeatherForecast forecast);
```
Exception `NotSupportedNClientException` will be thrown.

#### Dictionaries of non primitive types in query:
```ruby
[GetMethod]
void Get([QueryParam] Dictionary<int, WeatherForecast> forecasts);
```
Exception `NotSupportedNClientException` will be thrown.

#### Arrays of non primitive types in query:
```ruby
[GetMethod]
void Get([QueryParam] WeatherForecast[] forecasts);
```
Exception `NotSupportedNClientException` will be thrown.
