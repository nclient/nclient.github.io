# Asynchronously calls
NClient can work both synchronously and asynchronously. To execute a request to the service asynchronously, 
you should specify the returned type as `Task` or `Task<>`:

```ruby
public interface IProductServiceClient
{
    [GetMethod]
    Product Get(int id);             // sync call
    [GetMethod]
    Task<Product> GetAsync(int id);  // async call

    [PostMethod]
    void Post(Product product);      // sync call
    [PostMethod]
    Task PostAsync(Product product); // async call
}
```
