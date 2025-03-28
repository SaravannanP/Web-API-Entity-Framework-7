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
```csharp 
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
```

Contains the app services and is a reusable component that provides 
app functionality 

Services are registerd here to be consumed in the web service via dependency injection 

This class creates apps request processing pipeline
- Specify how the app responds to HTTP requests
  
The .Use extension methods (UseHttpsRedirection(),UseAuthorization()) signify adding middleware components to request pipeline.


### WeatherForecastController.cs

```csharp 
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
```

NOTE: Routing attribute Route which defines how to access the controller

In chrome press F12 > network > Fetch/XHR ( XHR = XML HTTP Request)

Execute the controller in swagger and observe the response in preview 



## Section 2: Video 8 - The Model-View-Controller (MVC) Pattern

MVC is a software design pattern into 3 interconnected elements.

Model = data 


Example : 
```csharp 
    public class Character 
    {
        public int Id { get; set; }
        // initialised by default to Frodo 
        // If no default value for name
        // public string? Name  {get;set} (For non nullable property <nullable> in .csproj) 
        public string Name { get; set; } = "Frodo";
        public int HitPoints { get; set; } = 100;
        
        public int Strength { get; set; } = 10;
        
        public int Defense { get; set; } = 10;
        
        public int Intelligence { get; set; } = 10;
        
    }
```
View = UI (User Interface)

model updates the view 

Controller - does the actual work 

Get image from Section 2 lesson 8. The Model-View-Controller(MVC) Pattern


## Section 2 : Video 9 - New Models

### Create a Models folder 
- Create a cs class called Character.js 
- Create some properties 

### Character.cs 
```csharp 
    namespace WebApplication2.Models
    {
        public class Character
        {
            public int Id { get; set; }
            // initialised by default to Frodo 
            // If no default value for name
            // public string? Name  {get;set} (For non nullable property <nullable> in .csproj) 
            public string Name { get; set; } = "Frodo";
            public int HitPoints { get; set; } = 100;
    
            public int Strength { get; set; } = 10;
    
            public int Defense { get; set; } = 10;
    
            public int Intelligence { get; set; } = 10;
        }
    }
```
### Under Models folder
- Create RPGClass.cs for enum to be used as class property in Character.cs  

### RPGClass.cs 

```csharp 
    namespace WebApplication2.Models
    {
        public enum RpgClass
        {
            Knight = 1,
    
            Mage = 2, 
    
            Cleric = 3,
        }
    }
```

### Under Models folder 
- Upadate Chracter.cs accordingly and set default class property 

### Character.cs 

```csharp 
    namespace WebApplication2.Models
    {
        public class Character
        {
            public int Id { get; set; }
            // initialised by default to Frodo 
            // If no default value for name
            // public string? Name  {get;set} (For non nullable property <nullable> in .csproj) 
            public string Name { get; set; } = "Frodo";
            public int HitPoints { get; set; } = 100;
    
            public int Strength { get; set; } = 10;
    
            public int Defense { get; set; } = 10;
    
            public int Intelligence { get; set; } = 10;
    
    
            // NEW RPG class property for that an Enum is required 
            // Created RPGClass.cs in Models folder first 
            // Set to Knight as default 
            public RpgClass RpgClass { get; set; } = RpgClass.Knight;
    
        }
    }
```
## Section 2 : Video 10 - New Controller & GET a New Character
### Under Controller Folder 
- Create CharacterController.cs with no View support(Derive from ControllerBase)
- Ensure Microsoft.ASPNetCore.MVC reference which is required for controller classes 

### ChracterController.cs 

```csharp 
    using Microsoft.AspNetCore.Mvc;// Reference for Controller class 

    namespace WebApplication2.Controllers
    {
        // NOTE: Deriving from Controller class mean there is View support 
        //       Deriving from ControllerBase class means no View support 
        public class CharacterController : ControllerBase
        {
            
        }
    }
```
### Under controller Folder 
- Add attributes to CharacterController.cs 
- Api Controller attribute 
- Route attribute


### CharacterController.cs
```csharp 
    using Microsoft.AspNetCore.Mvc;// Reference for Controller class 

    namespace WebApplication2.Controllers
    {
        // NOTE: Deriving from Controller class mean there is View support 
        //       Deriving from ControllerBase class means no View support 
    
        [ApiController] // Enables attribute routing and automatic HTTP 400 responses
        [Route("api/[controller]")] // enables sourcing for specific controller (api/Character)
        public class CharacterController : ControllerBase
        {
            
        }
    }
```
### Under controller Folder 
- Add RPG models reference to CharacterController.cs 

### CharacterController.cs 

```csharp 
    using Microsoft.AspNetCore.Mvc;// Reference for Controller class
    // can include directive at the top like (global using WebApplication2.Models;)
    // or can be placed in Program.cs (global using  WebApplication2.Models;)
    // or can create a seperate class just for using directives 
    // using WebApplication2.Models;// Reference Character class 
    
    
    namespace WebApplication2.Controllers
    {
        // NOTE: Deriving from Controller class mean there is View support 
        //       Deriving from ControllerBase class means no View support 

    [ApiController] // Enables attribute routing and automatic HTTP 400 responses
    [Route("api/[controller]")] // enables sourcing for specific controller (api/Character)
    public class CharacterController : ControllerBase
    {
        // Add RPG models referece  
        private static Character knight = new Character();
    }
    }

```
### Under Controller Folder 
- Add HTTP Get method 
    
NOTE: [HttpGet] is required else there will be a failed load API definition 

### CharacterController.cs 

```csharp            
    using Microsoft.AspNetCore.Mvc;// Reference for Controller class
    // can include directive at the top like (global using WebApplication2.Models;)
    // or can be placed in Program.cs (global using  WebApplication2.Models;)
    // or can create a seperate class just for using directives 
    // using WebApplication2.Models;// Reference Character class 

    namespace WebApplication2.Controllers
    {
        // NOTE: Deriving from Controller class mean there is View support 
        //       Deriving from ControllerBase class means no View support 
    
        [ApiController] // Enables attribute routing and automatic HTTP 400 responses
        [Route("api/[controller]")] // enables sourcing for specific controller (api/Character)
        public class CharacterController : ControllerBase
        {
            // Add RPG models referece  
            private static Character knight = new Character();
    
            // Implment get method to recieve game character
            // IActionResult enables forwarding of specific HTTP codes to client with data requested 
    
            [HttpGet] // Indicates method meant to handle HTTP GET requests 
            public IActionResult Get()
            {
                // Status code 200 with mock Character 
                // return BadRequest(Knight); 400
                // return NotFound(Knight); 404 
                // return NotAllowed(Knight); 405 
                // return Ok(new {message = "Hello world"}); 
                return Ok(knight);
            }
        }
    }
```
NOTE: Schemas Wont appear in swagger 

### Under Controller Folder 
- Update Get method from IActionResult to ActionResult for schema in swagger 


### CharacterController.cs

```csharp
using Microsoft.AspNetCore.Mvc;// Reference for Controller class
// can include directive at the top like (global using WebApplication2.Models;)
// or can be placed in Program.cs (global using  WebApplication2.Models;)
// or can create a seperate class just for using directives 
// using WebApplication2.Models;// Reference Character class 


namespace WebApplication2.Controllers
{
    // NOTE: Deriving from Controller class mean there is View support 
    //       Deriving from ControllerBase class means no View support 

    [ApiController] // Enables attribute routing and automatic HTTP 400 responses
    [Route("api/[controller]")] // enables sourcing for specific controller (api/Character)
    public class CharacterController : ControllerBase
    {
        // Add RPG models referece  
        private static Character knight = new Character();

        // Implment get method to recieve game character
        // IActionResult enables forwarding of specific HTTP codes to client with data requested 

        [HttpGet] // Indicates method meant to handle HTTP GET requests 
        public ActionResult<Character> Get() // To display schemas in swagger 
        {
            // Status code 200 with mock Character 
            // return BadRequest(Knight); 400
            // return NotFound(Knight); 404 
            // return NotAllowed(Knight); 405 
            // return Ok(new {message = "Hello world"}); 
            return Ok(knight);
        }
    }
}
```

### Under Models Folder 
- Update RPGclass.cs with [JsonConverter(typeof(JsonStringEnumConverter))] attribute

### RPGClass.cs 
```csharp
using System.Text.Json.Serialization; // directive added for Json Serialization 

namespace WebApplication2.Models
{
    // Since only numbers are shown instead of RPG class name in the swagger schema
    // Observe in swagger schema RPGclass that knight,mage,Cleric is seen in enum  
    [JsonConverter(typeof(JsonStringEnumConverter))]
    public enum RpgClass
    {
        Knight = 1,

        Mage = 2, 

        Cleric = 3,
    }
}
```
## Section 2: Video 11 - First steps with attribute routing  
### Under controller Folder
- Update private static character to return a list
- Update get method to return a singular character or all character

### CharacterController.cs 

```csharp
using Microsoft.AspNetCore.Mvc;// Reference for Controller class
// can include directive at the top like (global using WebApplication2.Models;)
// or can be placed in Program.cs (global using  WebApplication2.Models;)
// or can create a seperate class just for using directives 
// using WebApplication2.Models;// Reference Character class 


namespace WebApplication2.Controllers
{
    // NOTE: Deriving from Controller class mean there is View support 
    //       Deriving from ControllerBase class means no View support 

    [ApiController] // Enables attribute routing and automatic HTTP 400 responses
    [Route("api/[controller]")] // enables sourcing for specific controller (api/Character)
    public class CharacterController : ControllerBase
    {
        // Add RPG models reference  
        // private static Character knight = new Character();
        private static List<Character> characters = new List<Character>
        {
            // Object initialize 
            new Character(),
            new Character{ Name = "Sam"}
        };

        // since returning complete list of characters return type has to be an actionresult of characters
        [HttpGet("GetAll")]
        public ActionResult <List<Character>> Get()
        {
            return Ok(characters);
        }

        [HttpGet]
        public ActionResult<Character> GetSingle()
        {
            return Ok(characters[0]);
        }
    }
}


```

## Section 2: Video 12 - Routing with parameters 

### CharacterController.cs 

```csharp

using Microsoft.AspNetCore.Mvc;// Reference for Controller class
// can include directive at the top like (global using WebApplication2.Models;)
// or can be placed in Program.cs (global using  WebApplication2.Models;)
// or can create a seperate class just for using directives 
// using WebApplication2.Models;// Reference Character class 


namespace WebApplication2.Controllers
{
    // NOTE: Deriving from Controller class mean there is View support 
    //       Deriving from ControllerBase class means no View support 

    [ApiController] // Enables attribute routing and automatic HTTP 400 responses
    [Route("api/[controller]")] // enables sourcing for specific controller (api/Character)
    public class CharacterController : ControllerBase
    {
        // Add RPG models reference  
        // private static Character knight = new Character();
        private static List<Character> characters = new List<Character>
        {
            // Object initialize 
            new Character(),
            new Character{ Id = 1,Name = "Sam"}
        };

        // since returning complete list of characters return type has to be an actionresult of characters
        [HttpGet("GetAll")]
        public ActionResult <List<Character>> Get()
        {
            return Ok(characters);
        }


        // The web service knows the id is the value of the actual Id parameter 
        [HttpGet("{id}")]
        // This method returns first character where character equates to given Id
        public ActionResult<Character> GetSingle(int id)
        {
            // first or deafult and lambda expression
            // c for character 
            return Ok(characters.FirstOrDefault(c => c.Id == id));
        }
    }
}

```
## Section 2 : Video 13 - HTTP Request Methods Explained

HTTP = Hypertext Transfer Protocol 
- Set of request methods indicating desire action for given resource
- 9 request methods but GET,POST,PUT,DELETE most common

HTTP Request Methods 
- GET method request representation of specified resource (recieve)                        -READ
- POST method submits entity to resource causing change in state or server side effects (add or create new obj) -CREATE
- PUT method replaces all current representations of resource with request payload (update object and soft deletes) -UPDATE 
- DELETE method delete resource -DELETE
  
## Section 2 : Video 14 - Add a New Character with POST

Creating a new controller method to add a new character 
- Client sends JSON obj to service > service creates new character based on JSON data

 

