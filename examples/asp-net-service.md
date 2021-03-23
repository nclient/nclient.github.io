# Client for ASP.NET service
If you want to generate a client for a ASP.NET web service, then you need to extract an interface for your controller 
and annotate it with attributes from `NClient.Core.Attributes`. They are very similar to attributes for ASP.NET controllers. Follow the steps below:
#### Step 1: Create controller
```C#
public class WeatherForecastController : ControllerBase
{
    public async Task<WeatherForecast> GetAsync(DateTime date) =>
        new WeatherForecast(date: date, temperatureC: -25);
}
```
Note that you don't need to annotate it with ASP.NET attributes.
#### Step 2: Extract interface for your controller and inherit `INClient` interface
```C#
[Path("[controller]")] // equivalent to [ApiController, Route("[controller]")]
public interface IWeatherForecastController : INClient
{
    [GetMethod] // equivalent to [HttpGet]
    Task<WeatherForecast> GetAsync([QueryParam] DateTime date); // equivalent to [FromQuery]
}

public class WeatherForecastController : ControllerBase, IWeatherForecastController { ... }
```
The annotation in the interface instead of the controller allows you to put the interface in a separate assembly. 
Therefore, the client that will use this interface will not depend on the service.
If you want to use native attributes, see [Client for native ASP.NET service](././native-asp-net-service)  
#### Step 3 (optional): Create interface for client
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
This should be done if you want your client type to not contain "Сontroller" in the name.
#### Step 4: Create client
```C#
IWeatherForecastController client = new ClientProvider()
    .Use<IWeatherForecastController>(host: new Uri("http://localhost:8080"))
    .SetDefaultHttpClientProvider()
    .WithoutResiliencePolicy()
    .Build();
```
If you decide to follow the 3 step, use `IWeatherForecastClient` interface instead of `IWeatherForecastController`.
#### Step 5: Send an http request
```C#
// Equivalent to the following request: 
// curl -X GET -H "Content-type: application/json" http://localhost:8080/WeatherForecast?date=2021-03-13T00:15Z
var forecast = await client.GetAsync(DateTime.Now);
Console.WriteLine($"Date {forecast.Date}: {forecast.TemperatureC}°C");
```
