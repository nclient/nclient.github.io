# Http response
Sometimes it is necessary to get a full http response from a web service. In this case, you can use the standard NClient methods:

```ruby
[Api]
public interface IProductServiceClient : INClient
{
    [AsHttpGet]
    Task<Product> GetAsync(int id);
}

...

IProductServiceClient client = new ClientProvider()
    .Use<IProductServiceClient>(host: new Uri("http://localhost:8080"))
    .SetDefaultHttpClientProvider()
    .WithoutResiliencePolicy()
    .Build();

HttpResponse<Product> response = await client.AsHttp()
    .GetHttpResponse(productClient => productClient.GetAsync(id: 1));
```
