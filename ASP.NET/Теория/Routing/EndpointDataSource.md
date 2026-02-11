Информация о ендпоинтах

```C#
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.Map("/", () => "Index Page");
app.Map("/about", () => "About Page");
app.Map("/contact", () => "Contacts Page");

app.Map("/routes", async (IEnumerable<EndpointDataSource> endpoints, HttpContext context) =>
{
    await context.Response.WriteAsync(String.Join("\n", endpoints.SelectMany(e => e.Endpoints)));
});

app.Run();
```