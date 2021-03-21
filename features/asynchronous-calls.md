# Asynchronously calls
NClient can work both synchronously and asynchronously. To execute a request to the service asynchronously, 
you should specify the returned type as `Task` or `Task<>`:

```ruby
[Api]
public interface IProductServiceClient : INClient
{
    [AsHttpGet]
    Product Get(int id);             // sync call
    [AsHttpGet]
    Task<Product> GetAsync(int id);  // async call

    [AsHttpPost]
    void Post(Product product);      // sync call
    [AsHttpPost]
    Task PostAsync(Product product); // async call
}
```
