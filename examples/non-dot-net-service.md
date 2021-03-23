# Client for non .Net service

If you do not have the source code of the ASP.NET web service or you want to send requests to the non .Net service, 
then you just need to create an interface that describes the service you want to make requests to. Follow the steps below:

#### Step 1: Create interface for your service and inherit `INClient` interface
```ruby
[Path("api")]
public interface IProductServiceClient : INClient
{
    [PostMethod("products")]
    Task PostAsync([BodyParam] Product product);
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
// Equivalent to the following request: 
// curl -X POST -H "Content-type: application/json" --data "{ id: 1 }" http://localhost:8080/api/products
await client.PostAsync(new Product(id: 1));
```
