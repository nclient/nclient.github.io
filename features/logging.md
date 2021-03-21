# Logging
You can optionally specify a logger for a client. NClient does not provide a default implementation of the logger, 
but you can use any implementation of `Microsoft.Extensions.Logging.ILogger<TCategoryName>` interface 
([see Microsoft docs](docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.logging.ilogger-1)).

```ruby
ILogger<IProductServiceClient> logger = ...;

IProductServiceClient client = new ClientProvider()
    .Use<IProductServiceClient>(host: new Uri("http://localhost:8080"))
    .SetDefaultHttpClientProvider()
    .WithoutResiliencePolicy(policy)
    .WithLogger(logger)
    .Build();
```
