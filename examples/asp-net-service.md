# Client for ASP.NET service
If you want to generate a client for a ASP.NET web service, then you need to extract an interface for your controller and annotate it with attributes from `NClient.Annotations`. They are very similar to attributes for ASP.NET controllers. Follow the steps below:
#### Step 1: Install `NClient.AspNetCore` on server-side
```
dotnet add package NClient.AspNetCore
```
#### Step 2: Create controller
```C#
public class WeatherForecastController : ControllerBase
{
    public async Task<WeatherForecast> GetAsync(DateTime date) =>
        new WeatherForecast(date: date, temperatureC: -25);
}
```
Note that you don't need to annotate it with ASP.NET attributes.
#### Step 3: Extract interface for your controller
```C#
[Path("[controller]")]                                            // equivalent to [ApiController, Route("[controller]")]
public interface IWeatherForecastController
{
    [GetMethod]                                                   // equivalent to [HttpGet]
    Task<WeatherForecast> GetAsync([QueryParam] DateTime date);   // equivalent to [FromQuery]
}

public class WeatherForecastController : ControllerBase, IWeatherForecastController { ... }
```
The annotation in the interface instead of the controller allows you to put the interface in a separate assembly. 
Therefore, the client that will use this interface will not depend on the service.
#### Step 4 (optional): Create interface for client
```C#
public interface IWeatherForecastClient : IWeatherForecastController, INClient
{
}

[Path("[controller]")]
public interface IWeatherForecastController
{
    [GetMethod]
    Task<WeatherForecast> GetAsync([QueryParam] DateTime date);
}

public class WeatherForecastController : ControllerBase, IWeatherForecastController { ... }
```
This should be done if you want your client type to not contain "Сontroller" in the name. If you add `INClient` interface, you will get additional NClient features: receive a full http response and change a resilience policy for requests.
#### Step 6: Add controller to ServiceCollection in Startup.cs
```C#
public void ConfigureServices(IServiceCollection services)
{
    // ...
    services.AddNClientControllers();
}
```
`AddNClientControllers` method can be used in combination with `AddControllers`.
#### Step 7: Install `NClient` on client-side
```
dotnet add package NClient
```
#### Step 8: Create client
```C#
IWeatherForecastController client = NClientProvider
    .Use<IWeatherForecastController>(host: "http://localhost:8080")
    .Build();
```
If you decide to follow the 4 step, use `IWeatherForecastClient` interface instead of `IWeatherForecastController`.
#### Step 9: Send an http request
```C#
// Equivalent to the following request: 
// curl -X GET -H "Content-type: application/json" http://localhost:8080/WeatherForecast?date=2021-03-13T00:15Z
var forecast = await client.GetAsync(DateTime.Now);
Console.WriteLine($"Date {forecast.Date}: {forecast.TemperatureC}°C");
```
