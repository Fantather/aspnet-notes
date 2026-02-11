```C#
if (app.Environment.IsDevelopment())

{

    app.Run(async (context) => await context.Response.WriteAsync("In Development Stage"));

}

else

{

    app.Run(async (context) => await context.Response.WriteAsync("In Production Stage"));

}

Console.WriteLine($"{app.Environment.EnvironmentName}");
```