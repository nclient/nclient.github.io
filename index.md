# NClient
![Nuget](https://img.shields.io/nuget/v/NClient)
![GitHub last commit](https://img.shields.io/github/last-commit/nclient/nclient)
![GitHub Workflow Status](https://img.shields.io/github/workflow/status/nclient/nclient/Test)

NClient is an HTTP client that allows you to call web service API methods through annotated controllers or interfaces. 
The client supports asynchronous calls, retry policies and logging. All this is  simple and flexible to configure.

## Why use NClient?
Creating clients for web services can be quite a challenge because, in addition to data transfer, you need to implement query building, 
serialization, retry policy, error handling, logging — and this is not to mention the maintenance that comes with each update of your APIs. 
What if you could create clients with a fraction of the effort? This is exactly what NClient hopes to achieve by allowing you to create 
clients declaratively.

## How do I use NClient?
To generate a client, you just need to create an interface describing available endpoints and input/output data. After that, you can generate 
and configure the client, using the `ClientProvider`.

### Usage with ASP.NET Core
If you want to generate a client for a ASP.NET web service, then you don't even have to add attributes to the service interface. 
Everything you need:

#### Step 1: Create controller
```ruby
[ApiController, Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public async Task<WeatherForecast> GetAsync(DateTime date) =>
        new WeatherForecast(date: date, temperatureC: -25);
}
```

#### Step 2: Extract interface for your controller and add `INClient` interface
```ruby
public interface IWeatherForecastClient : INClient
{
    Task<WeatherForecast> GetAsync(DateTime date);
}

[ApiController, Route("[controller]")]
public class WeatherForecastController : ControllerBase, IWeatherForecastClient { ... }
```
#### Step 3: Create client
```ruby
IWeatherForecastClient client = new AspNetClientProvider()
    .Use<IWeatherForecastClient, WeatherForecastController>(host: new Uri("http://localhost:8080"))
    .SetDefaultHttpClientProvider()
    .WithoutResiliencePolicy()
    .Build();
```

#### Step 4: Send an http request
```ruby
var forecast = await client.GetAsync(DateTime.Now);
Console.WriteLine($"Date {forecast.Date}: {forecast.TemperatureC}°C");
```
This is equivalent to the following request: `curl -X GET -H "Content-type: application/json" 
http://localhost:8080/WeatherForecast?date=2021-03-13T00:15Z`

### Usage with non .Net web service
If you do not have the source code of the ASP.NET web service or you want to send requests to the non .Net service, 
then you need to create an interface and additionally declare it with attributes from `NClient.InterfaceProxy.Attributes`. 
They are very similar to attributes for ASP.NET controllers. Follow the steps below:

#### Step 1: Create interface for your service and add `INClient` interface
```ruby
[Api(template: "api")]
public interface IProductServiceClient : INClient
{
    [AsHttpPost(template: "products")]
    Task PostAsync([ToBody] Product product);
}
```

#### Step 2: Create client
```ruby
IProductServiceClient client = new ClientProvider()
    .Use<IProductServiceClient>(host: new Uri("http://localhost:8080"))
    .SetDefaultHttpClientProvider()
    .WithoutResiliencePolicy()
    .Build();
```

#### Step 3: Send an http request
```ruby
await client.PostAsync(new Product(id: 1));
```
This is equivalent to the following request: `curl -X POST -H "Content-type: application/json" --data "{ id: 1 }" 
http://localhost:8080/api/products`
