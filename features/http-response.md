# Http response
Sometimes it is necessary to get a full http response from a web service. In this case, you can use the standard NClient methods:

```ruby
public interface IProductServiceClient : INClient
{
    [GetMethod]
    Task<Product> GetAsync(int id);
}

...

IProductServiceClient client = new ClientProvider()
    .Use<IProductServiceClient>(host: new Uri("http://localhost:8080"))
    .Build();

HttpResponse<Product> response = await client.AsHttp()
    .GetHttpResponse(productClient => productClient.GetAsync(id: 1));
```
