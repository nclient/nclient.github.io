# Client for native ASP.NET service
> Note that you will have strong cohesion between the client and the service if you use the controller to create the client. 
> To avoid this, use the interface as in [this example](././asp-net-service).

If you want to generate a client for a ASP.NET web service, then you don't even have to add attributes to the service interface. Everything you need:

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
// Equivalent to the following request: 
// curl -X GET -H "Content-type: application/json" http://localhost:8080/WeatherForecast?date=2021-03-13T00:15Z
var forecast = await client.GetAsync(DateTime.Now);
Console.WriteLine($"Date {forecast.Date}: {forecast.TemperatureC}°C");
```
 
