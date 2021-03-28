# Controller-based client

A client for a ASP.NET web service can be created based on information about its controller. 
All you need to do is extract the interface for the controller with the controller methods you need:

```ruby
public interface IWeatherForecastClient : INClient
{
    Task<WeatherForecast> GetAsync(DateTime date);
}

[ApiController, Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public async Task<WeatherForecast> GetAsync(DateTime date) =>
        new WeatherForecast(date: date, temperatureC: -25);
}
``` 

> If you add `INClient` interface, you will get additional NClient features: receive a full http response and change a resilience policy for requests (see [features](/features/index.md)).

Now that you have a controller and an interface for the client, you can create a client:

```ruby
IWeatherForecastClient client = NClientProvider
    .Use<IWeatherForecastClient, WeatherForecastController>(host: "http://localhost:8080")
    .Build();
```

### Limitations
NClient supports most of the attributes applied to controllers, but with some limitations:

#### Multiple attributes:
```ruby
[HttpGet("weather"), HttpGet("weather/{date}")]
public WeatherForecast GetAsync(DateTime date) { ... }
```
Exception `NotSupportedNClientException` will be thrown.

#### Multiple body parameters:
```ruby
[HttpPost]
public void Post([FromBody] WeatherForecast forecast, [FromBody] DateTime date) { ... }
```
Exception `NotSupportedNClientException` will be thrown.

#### Route attribute for methods:
```ruby
[HttpPost, Route("weather")]
public void Post([FromBody] WeatherForecast forecast) { ... }
```
Exception `NotSupportedNClientException` will be thrown.

#### Non primitive types in route template:
```ruby
[HttpPost("weather/{forecast}")]
public void PostAsync(WeatherForecast forecast) { ... }
```
Exception `NotSupportedNClientException` will be thrown.

#### Non primitive types in headers:
```ruby
[HttpPost]
public void Post([FromHeader] WeatherForecast forecast) { ... }
```
Exception `NotSupportedNClientException` will be thrown.

#### Dictionaries of non primitive types in query:
```ruby
[HttpGet]
public void Get([FromQuery] Dictionary<int, WeatherForecast> forecasts) { ... }
```
Exception `NotSupportedNClientException` will be thrown.

#### Arrays of non primitive types in query:
```ruby
[HttpGet]
public void Get([FromQuery] WeatherForecast[] forecasts) { ... }
```
Exception `NotSupportedNClientException` will be thrown.
