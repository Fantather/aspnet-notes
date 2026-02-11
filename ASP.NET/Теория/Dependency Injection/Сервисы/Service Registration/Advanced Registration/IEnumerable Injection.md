

```C#
using System.Diagnostics;

var builder = WebApplication.CreateBuilder();
builder.Services.AddSingleton<ILogger, ConsoleLogger>();
builder.Services.AddSingleton<ILogger, DBLogger>();

var app = builder.Build();

app.UseMiddleware<ContentMiddleware>();

app.Run();

class ContentMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IEnumerable<ILogger> _loggers;

    public ContentMiddleware(RequestDelegate next, IEnumerable<ILogger> loggers)
    {
        _next = next;
        _loggers = loggers;

    }
    public async Task InvokeAsync(HttpContext context)
    {
        if (context.Request.Path == "/")
        {
            LogMessage($"Visited page: 'Index'. Date: {DateTime.Now}");
            await context.Response.WriteAsync("Index Page");
        }
        else if (context.Request.Path == "/about")
        {
            LogMessage($"Visited page: 'About'. Date: {DateTime.Now}");
            await context.Response.WriteAsync("About Page");
        }
        else
        {
            LogMessage($"Moving on to the next one Middleware. Date: {DateTime.Now}");
            await _next.Invoke(context);
        }
    }

    private void LogMessage(string message)
    {
        foreach (var logger in _loggers)
        {
            logger.Log(message);
        }
    }
}

interface ILogger
{
    void Log(string message);
}

class ConsoleLogger : ILogger
{
    public void Log(string message)
    {
        Console.WriteLine(message);
    }
}

class DBLogger : ILogger
{
    public void Log(string message)
    {
        Debug.WriteLine(message);
    }
}
```