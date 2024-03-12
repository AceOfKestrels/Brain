# Web APIs using ASP\.NET Core

RESTful APIs can be created using the ASP\.NET Core framework in C#.

## Creating a basic API Project
A new API project can easily be created using Visual Studio. Select `ASP.NET Core-Web-API` as the project template. 
Other configuration options include
- Enabling HTTPS
- Docker support
- OpenAPI support
- usage of Controllers or Minimal API

This will generate a new project with a `WeatherForecase` example model and controller. You can refer to those to get the basic idea.

## Creating the API Controller
Add a new C# file (in the Controllers folder) or use the `Controller` template. The controller class name should end with `Controller` by convention and the class should extend `ControllerBase`. 

It is decorated with the `ApiController` and `Route` attributes. The latter defines the URL path of this API endpoint. Setting this to `[controller]` will use the name of the controller (the start of the class name before "Controller").

Example controller classes:
```cs
[ApiController]
[Route("users")] //[Route("[controller]")] will make the route "user" here
public class UserController: ControllerBase
{
}
```

## HTTP Action Methods
In this class the methods reacting to incoming HTTP requests are defined. 
They should return an `ActionResult` result with the generic type being the model the response should return. The methods can also be declared as asynchronous if they involve any async calls, in which case they should return `Task<ActionResult>`.

They are decorated with attributes such as `HttpGet` or `HttpPut`. Another route *after* the controller's path can be defined either as an argument to this attribute or again using a separate `Route` attribute:
```cs
[ApiController]
[Route("users")]
public class UserController: ControllerBase
{
    [HttpGet("list")] // /users/list
    public async Task<ActionResult<IEnumerable<User>>> GetUsers() {
        return Ok(_userService.Users);
    }
}
```

### Parameters
Route parameters can be defined using curly brackets. They will be available as arguments to the method:
```cs
[ApiController]
[Route("users")]
public class UserController: ControllerBase
{
    [HttpGet("{id}")] // /users/{id} e.g. /users/1  /users/5  etc.
    public async Task<ActionResult<User>> GetUser(int id) {
        return Ok(_userService.GetUser(id));
    }
}
```

### Body
Model data can be supplied in the request body. It can be accessed using a method argument with the `FromBody` attribute:
```cs
[ApiController]
[Route("users")]
public class UserController: ControllerBase
{
    [HttpPut]
    public async Task<ActionResult<User>> UpdateUser([FromBody] User user) {
        return Ok(_userService.UpdateUser(user));
    }
}
```