# Web-API-Entity-Framework-7
Build the back-end of a .NET 7 web application with a Web API, Entity Framework 7 &amp; SQL Server in no time!

## Section 1: Intro - .NET7 

After installing 

in integrated terminal in vscode:

    > donet
    > donet -h 
    > donet new 
    > dotnet new --list 
    > dotnet new webapi 
or use VS template: WEB api 

### Program.cs

      var builder = WebApplication.CreateBuilder(args);

      // Add services to the container.
      
      builder.Services.AddControllers();
      // Learn more about configuring Swagger/OpenAPI at  
      https://aka.ms/aspnetcore/swashbuckle
      builder.Services.AddEndpointsApiExplorer();
      builder.Services.AddSwaggerGen();
      
      var app = builder.Build();
      
      // Configure the HTTP request pipeline.
      if (app.Environment.IsDevelopment())
      {
          app.UseSwagger();
          app.UseSwaggerUI();
      }
      
      app.UseHttpsRedirection();
      
      app.UseAuthorization();
      
      app.MapControllers();
      
      app.Run();
Contains the app services and is a reusable component that provides 
app functionality 

Services are registerd here to be consumed in the web service via dependency injection 

This class creates apps request processing pipeline
  - Specify how the app responds to HTTP requests
The .Use extension methods (UseHttpsRedirection(),UseAuthorization()) signify adding middleware components to request pipeline.


### WeatherForecastController.cs

    using Microsoft.AspNetCore.Mvc;

    namespace WebApplication2.Controllers
    {
        [ApiController]
        [Route("[controller]")]
        public class WeatherForecastController : ControllerBase
        {
            private static readonly string[] Summaries = new[]
            {
                "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
            };

        private readonly ILogger<WeatherForecastController> _logger;

        public WeatherForecastController(ILogger<WeatherForecastController> logger)
        {
            _logger = logger;
        }

        [HttpGet(Name = "GetWeatherForecast")]
        public IEnumerable<WeatherForecast> Get()
        {
            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = DateOnly.FromDateTime(DateTime.Now.AddDays(index)),
                TemperatureC = Random.Shared.Next(-20, 55),
                Summary = Summaries[Random.Shared.Next(Summaries.Length)]
            })
            .ToArray();
          }
        }
     }

NOTE: Routing attribute Route which defines how to access the controller

In chrome press F12 > network > Fetch/XHR ( XHR = XML HTTP Request) 
Execute the controler in swagger and observe the response in preview 



## Section 2: Web API-.NET7
MVC is a software design pattern into 3 interconnected elements.
Model = data 

Example : 

    public class Character 
    {
        public int Id {get; set;} = 0;
        public string Name {get;set;} = "Frodo";
        public int HitPoints {get;set;} = 100;
    }

View = UI (User Interface) 
model updates the view 

Controller - does the actual work 

Get image from Section 2 lesson 8. The Model-View-Controller(MVC) Pattern


