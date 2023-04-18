## Код

### Dependency Injection



Web/DependencyInjection.cs

~~~ csharp
        public static IServiceCollection AddPresentation(this IServiceCollection services)
        {
            services.AddIdentity<IdentityUser, IdentityRole>(options =>
            {
                options.Password.RequireDigit = false;
                options.Password.RequireNonAlphanumeric = false;
                options.Password.RequireUppercase = false;
                options.Password.RequiredLength = 6;
            })
    .AddRoles<IdentityRole>()
    .AddEntityFrameworkStores<ApplicationDbContext>();

            services.AddSwaggerGen(x =>
            {
                x.SwaggerDoc("v1", new Microsoft.OpenApi.Models.OpenApiInfo {Title="My Api", Version="v1" });
            });
            return services;
        }
~~~



<details>

<summary>Infrastructure/DependencyInjection.cs</summary>
      
``` csharp
public static IServiceCollection AddInfrastructure(this IServiceCollection services, ConfigurationManager config)
{
    return services;
}
```

</details>

<details>

<summary>Application/DependencyInjection.cs</summary>
      
```csharp
public static IServiceCollection AddApplication(this IServiceCollection services)
{
    services.AddMediatR(typeof(DependencyInjection).Assembly);
    return services;
}
```

</details>

<details>

<summary>Web/Program.cs</summary>

```csharp
var builder = WebApplication.CreateBuilder(args);
{
    builder.Services.AddApplication();
    builder.Services.AddInfrastructure(builder.Configuration);
    builder.Services.AddPresentation();
    builder.Services.AddControllers();
}

var app = builder.Build();
{
    app.UseHttpsRedirection();
    
    Assembly presentationAssembly = typeof(Presentation.AssemblyReference).Assembly;
    app.MapControllers().AddApplicationPart(presentationAssembly);

    app.Run();
}
```

</details>
  
### DateTime Provider
**Application**
<details>
<summary>Common/Services/IDateTimeProvider.cs</summary>
      
```csharp
public interface IDateTimeProvider
{
    DateTime UtcNow { get; }
}
```

</details>

**Infrastructure**

<details>

<summary>Services/DateTimeProvider.cs</summary>
      
```csharp
public class DateTimeProvider : IDateTimeProvider
{
    public DateTime UtcNow => DateTime.UtcNow;
}
```

</details>

<details>

<summary>DependencyInjection.cs</summary>
Добавить

```csharp
services.AddSingleton<IDateTimeProvider, DateTimeProvider>();
```

</details>

### DbContext & Unit of Work

**Web**

<details>

<summary>appsettings.json</summary>
      
```json
    "ConnectionStrings": {
    "DefaultConnection": "Data Source=(localdb)\\MSSQLLocalDB;Initial Catalog=Portfolio2022;Integrated Security=True;
    Connect Timeout=30;Encrypt=False;TrustServerCertificate=False;ApplicationIntent=ReadWrite;MultiSubnetFailover=False"
    }
```

</details>

**Infrastructure**

<details>

<summary>Persistence/ApplicationDbContext.cs</summary>
      
```csharp
public class ApplicationDbContext : IdentityDbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
    {

    }

    public DbSet<User> Users { get; set; }

    public DbSet<Post> Posts { get; set; }
}
```

</details>  
      
<details>

<summary>Persistence/UnitOfWork.cs</summary>
      
```csharp
public sealed class UnitOfWork : IUnitOfWork
{
    private readonly ApplicationDbContext _ctx;

    public UnitOfWork(ApplicationDbContext ctx)
    {
        _ctx = ctx;
    }

    public Task SaveChangesAsync(CancellationToken cancellationToken = default)
    {
        return _ctx.SaveChangesAsync(cancellationToken);
    }
}
```
</details>

<details>
<summary>DependencyInjection.cs</summary>
      
```csharp
public static IServiceCollection AddInfrastructure(this IServiceCollection services, IConfiguration configuration)
{
    services.AddDbContext<ApplicationDbContext>(options => options.UseSqlServer(configuration["DefaultConnection"]));
}
```
      
</details>
      
**Application**
<details>
<summary>Persistence/IUnitOfWork.cs</summary>
      
```csharp

```

</details>
      
### Identity
**Web**
<details>

<summary>Program.cs</summary>
      
```csharp
    var builder = WebApplication.CreateBuilder(args);
    {
        builder.Services.AddIdentity<IdentityUser, IdentityRole>(options =>
        {
            options.Password.RequireDigit = false;
            options.Password.RequireNonAlphanumeric = false;
            options.Password.RequireUppercase = false;
            options.Password.RequiredLength = 6;
        })
            .AddRoles<IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
    }
    var app = builder.Build();
    {
        {
            IServiceScope scope = app.Services.CreateScope();
            ApplicationDbContext ctx = scope.ServiceProvider.GetRequiredService<ApplicationDbContext>();
            UserManager<IdentityUser> userManager = scope.ServiceProvider.GetRequiredService<UserManager<IdentityUser>>();
            RoleManager<IdentityRole> roleManager = scope.ServiceProvider.GetRequiredService<RoleManager<IdentityRole>>();

            ctx.Database.EnsureCreated();

            IdentityRole adminRole = new IdentityRole("Admin");
            if (!ctx.Roles.Any())
            {
                roleManager.CreateAsync(adminRole).GetAwaiter().GetResult();
            }

            if (!ctx.Users.Any(u => u.UserName == "admin"))
            {
                IdentityUser adminUser = new IdentityUser
                {
                    UserName = "admin",
                    Email = "admin@example.com"
                };
                userManager.CreateAsync(adminUser, "P@ssword123!").GetAwaiter().GetResult();
                userManager.AddToRoleAsync(adminUser, adminRole.Name).GetAwaiter().GetResult();
            }        
        }
    }
```

</details>
      
### User Repository  

**Domain**

<details>

<summary>Aggregates/User.cs</summary>
      
```csharp
public class User
{
    public Guid Id { get; set; } = Guid.NewGuid();
    public string FirstName { get; set; } = null!;
    public string LastName { get; set; } = null!;
    public string Email { get; set; } = null!;
    public string Password { get; set; } = null!;
}
```

</details>  
      
**Application**

<details>

<summary>Persistence/IUserRepository.cs</summary>
      
```csharp
public interface IUserRepository
{
    User? GetUserByEmail(string email);
    void Add(User user);
}
```

</details>

**Infrastructure**

<details>

<summary>Persistence/UserRepository.cs</summary>
      
```csharp

public class UserRepository : IUserRepository
{
    private static readonly List<User> _users = new();

    public User? GetUserByEmail(string email)
    {
        return _users.SingleOrDefault(u => u.Email == email);
    }

    public void Add(User user)
    {
        _users.Add(user);
    }
}
```
</details>

<details>

<summary>DependencyInjection.cs</summary>
      
```csharp
public static class DependencyInjection
{
    public static IServiceCollection AddInfrastructure(this IServiceCollection services)
    {
        services.AddScoped<IUserRepository, UserRepository>();
        return services;
    }
}
```

</details> 

### Post Repository  

**Domain**

<details>

<summary>Entities/Post.cs</summary>
    
```csharp
    public class Post : Entity<int>
    {
        public DateTime CreationDate { get; set; }
        public string Author { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        public DateTime? DisplayUntil { get; set; }
        public string UserId { get; set; }
        public virtual ICollection<Tag> Tags { get; set; }
    }
```

</details>

<details>
<summary>Entities/Tag.cs</summary>

```csharp
    public class Tag : Entity<int>
    {
        public string Text { get; set; }

        public int? PostId { get; set; }

        [ForeignKey(nameof(PostId))]
        public Post Post { get; set; }
    }
```
</details>      
    
<details>
<summary>Entities/IPostService.cs</summary>
    
```csharp
    public interface IPostService : ITweetbookAppService<Post,int>
    {
        Task<IEnumerable<Tag>> GetTagsByPostAsync(int postId);
        Task<Tag> CreatePostTagAsync(Tag tag);
    }
```
</details>       
    
<details>
<summary>Entities/PostService.cs</summary>
    
```csharp
    public class PostService : TweetbookAppService<Post>, IPostService
    {
        //private readonly IHttpContextAccessor _httpContextAccessor;

        //public PostService(DataContext dataContext, IHttpContextAccessor httpContextAccessor) : base(dataContext)
        //{
        //    _httpContextAccessor = httpContextAccessor;
        //}

        public PostService(DataContext dataContext) : base(dataContext) {}

        public override async Task<bool> UpdateAsync(Post item)
        {
            CheckInstanceAvailability();

            var itemToUpdate = await GetByIdAsync(item.Id);
            if (itemToUpdate == null)
                return await Task.FromResult(false);

            //The infrastructure evaluates for us via PostOwnershipValidationFilter where applied
            //But what if the developer forgets to decorate the endpoint with the attribute aforementioned?
            //In this case, uncomment the line below or adopt a better coding strategy*
            //if (!CurrentUserIsOwner(itemToUpdate.UserId))
            //    return await Task.FromResult(false);
            //*throw new SecurityException("Access denied to the request resource or operation")

            //DataContext.Set<T>().Update(item); //See next line below

            //https://stackoverflow.com/questions/7106211/entity-framework-why-explicitly-set-entity-state-to-modified
            item.UserId = itemToUpdate.UserId;
            DataContext.Entry(itemToUpdate).CurrentValues.SetValues(item);

            var updated = await DataContext.SaveChangesAsync() > 0;

            return await Task.FromResult(updated);
        }

        public override async Task<bool> RemoveAsync(int id)
        {
            CheckInstanceAvailability();

            var itemToRemove = await GetByIdAsync(id);
            if (itemToRemove != null)
            {
                //The infrastructure evaluates for us via PostOwnershipValidationFilter where applied
                //But what if the developer forgets to decorate the endpoint with the attribute aforementioned?
                //In this case, uncomment the line below or adopt a better coding strategy*
                //if (!CurrentUserIsOwner(itemToRemove.UserId))
                //    return await Task.FromResult(false);
                //*throw new SecurityException("Access denied to the request resource or operation")

                DataContext.Entry(itemToRemove).State = EntityState.Deleted;
                await DataContext.SaveChangesAsync();
            }

            return await Task.FromResult(true);
        }

        public async Task<IEnumerable<Tag>> GetTagsByPostAsync(int postId)
        {
            //TODO: Implement get tags by post id method
            CheckInstanceAvailability();

            return await Task.FromResult(Enumerable.Empty<Tag>());
        }

        public async Task<Tag> CreatePostTagAsync(Tag tag)
        {
            CheckInstanceAvailability();

            var relatedPost = await GetByIdAsync(tag.PostId.Value);
            if (relatedPost != null)
            {
                await DataContext.Tags.AddAsync(tag);
                var created = await DataContext.SaveChangesAsync() > 0;

                return await Task.FromResult(created ? tag : null);
            }

            return await Task.FromResult((Tag)null);
        }

        //The infrastructure evaluates for us via PostOwnershipValidationFilter where applied
        //private bool CurrentUserIsOwner(string postUserId) => string.Equals(_httpContextAccessor.HttpContext.GetCurrentUserId(),
            postUserId, StringComparison.Ordinal);
    }
```
</details>   
 
<details>
<summary>Entities/IAppService.cs</summary>
      
```csharp
    public interface ITweetbookAppService<T, TKey> where T : class
    {
        Task<T> GetByIdAsync(TKey id);
        Task<IEnumerable<T>> GetAllAsync();
        Task<bool> CreateAsync(T item);
        Task<bool> UpdateAsync(T item);
        Task<bool> RemoveAsync(TKey id);
    }
```

</details>    
      
<details>
<summary>Entities/AppService.cs</summary>
    
```csharp
    public abstract class TweetbookAppService<T> : ITweetbookAppService<T, int> where T : Entity<int>
        {
            protected DataContext DataContext { get; private set; }

            public TweetbookAppService(DataContext dataContext)
            {
                DataContext = dataContext;
            }

            public async virtual Task<IEnumerable<T>> GetAllAsync()
            {
                CheckInstanceAvailability();

                return await Task.FromResult(DataContext.Set<T>().ToImmutableList());
            }

            public async virtual Task<T> GetByIdAsync(int id)
            {
                CheckInstanceAvailability();

                return await DataContext.Set<T>().FirstOrDefaultAsync(it => it.Id == id);
            }

            public async virtual Task<bool> CreateAsync(T item)
            {
                CheckInstanceAvailability();

                await DataContext.Set<T>().AddAsync(item);
                var created = await DataContext.SaveChangesAsync() > 0;

                return await Task.FromResult(created);
            }

            public async virtual Task<bool> RemoveAsync(int id)
            {
                CheckInstanceAvailability();

                var itemToRemove = await GetByIdAsync(id);
                if (itemToRemove != null)
                {
                    DataContext.Entry(itemToRemove).State = EntityState.Deleted;
                    await DataContext.SaveChangesAsync();
                }

                return await Task.FromResult(true);
            }

            public async virtual Task<bool> UpdateAsync(T item)
            {
                CheckInstanceAvailability();

                //DataContext.Set<T>().Update(item);

                var itemToUpdate = await DataContext.Set<T>().SingleOrDefaultAsync(it => it.Id == item.Id);
                if (itemToUpdate == null)
                    return await Task.FromResult(false);

                //https://stackoverflow.com/questions/7106211/entity-framework-why-explicitly-set-entity-state-to-modified
                DataContext.Entry(itemToUpdate).CurrentValues.SetValues(item);

                var updated = await DataContext.SaveChangesAsync() > 0;

                return await Task.FromResult(updated);
            }

            #region IDisposable Support
            private bool disposedValue = false; // To detect redundant calls

            protected virtual void Dispose(bool disposing)
            {
                if (!disposedValue)
                {
                    if (disposing)
                    {
                        if (DataContext != null)
                            DataContext.Dispose();
                    }

                    // TODO: free unmanaged resources (unmanaged objects) and override a finalizer below.
                    // TODO: set large fields to null.

                    disposedValue = true;
                }
            }

            ~TweetbookAppService()
            {
                Dispose(false);
            }

            public void Dispose()
            {
                Dispose(true);
                GC.SuppressFinalize(this);
            }
            #endregion

            protected void CheckInstanceAvailability()
            {
                if (disposedValue)
                    throw new ObjectDisposedException("This service instance was disposed and is no longer available!");
            }
        }
```
</details>     

### Authentication
**Web**

<details>

<summary>Contracts/v1/Authentication/Requests/RegisterRequest.cs</summary>

```csharp
public record RegisterRequest(
    string FirstName,
    string LastName,
    string Email,
    string Password);
```

</details>
<details>
<summary>Contracts/v1/Authentication/Requests/LoginRequest.cs</summary>

```csharp
public record LoginRequest(
    string Email,
    string Password);
```

</details>
<details>
<summary>Contracts/v1/Authentication/Responses/SuccessResponse.cs</summary>

```csharp
public record SuccessResponse(
    Guid Id,
    string FirstName,
    string LastName,
    string Email,
    string Token);
```
</details>
<details>
<summary>Contracts/v1/Authentication/Responses/FailedResponse.cs</summary>

```csharp
public class FailedResponse
{

}
```
</details>

**Application**
<details>
<summary>Authentication/Common/AuthenticationResult.cs</summary>
      
```csharp
public record AuthenticationResult(
    User User,
    string Token);
```
</details>    
<details>
<summary>Authentication/Commands/Register/RegisterCommand.cs</summary>
      
```csharp
public record RegisterCommand(
    string FirstName,
    string LastName,
    string Email,
    string Password) : IRequest<AuthenticationResult>;
```
</details>
      

<details>
<summary>Authentication/Commands/Register/RegisterCommandHandler.cs</summary>
      
```csharp
public class RegisterCommandHandler :
    IRequestHandler<RegisterCommand, AuthenticationResult>
{
    private readonly IJwtTokenGenerator _jwttokengenerator;
    private readonly IUserRepository _userRepository;

    public RegisterCommandHandler(IJwtTokenGenerator jwtTokenGenerator,
        IUserRepository userRepository)
    {
        _jwttokengenerator = jwtTokenGenerator;
        _userRepository = userRepository;
    }
    public async Task<AuthenticationResult> Handle(RegisterCommand command, CancellationToken cancellationToken)
    {
        if (_userRepository.GetUserByEmail(command.Email) is not null)
        {
            throw new Exception("User with given email already exists");
        }

        var user = new User
        {
            FirstName = command.FirstName,
            LastName = command.LastName,
            Email = command.Email,
            Password = command.Password
        };
        _userRepository.Add(user);

        Guid userId = Guid.NewGuid();
        var token = _jwttokengenerator.GenerateToken(user);

        return new AuthenticationResult(
            user,
            token);
    }
}
```
</details>
<details>
<summary>Authentication/Queries/Login/LoginQuery.cs</summary>
      
```csharp
public record LoginQuery(string Email, string Password)
        : IRequest<AuthenticationResult>;
```
</details>
<details>
<summary>Authentication/Queries/Login/LoginQueryHandler.cs</summary>
      
```csharp
public class LoginQueryHandler :
    IRequestHandler<LoginQuery, AuthenticationResult>
{
    private readonly IJwtTokenGenerator _jwttokengenerator;
    private readonly IUserRepository _userRepository;

    public LoginQueryHandler(IJwtTokenGenerator jwtTokenGenerator,
        IUserRepository userRepository)
    {
        _jwttokengenerator = jwtTokenGenerator;
        _userRepository = userRepository;
    }

    public async Task<AuthenticationResult> Handle(LoginQuery query, CancellationToken cancellationToken)
    {
        if (_userRepository.GetUserByEmail(query.Email) is not User user)
        {
            throw new Exception("User with given email does not exist");
        }

        if (user.Password != query.Password)
        {
            throw new Exception("Invalid password");
        }

        var token = _jwttokengenerator.GenerateToken(user);

        return new AuthenticationResult(
            user,
            token);
    }
}
```

</details>      
      
**Presentation**
      
<details>
<summary>Controllers/v1/AuthenticationController.cs</summary>
      
```csharp
[ApiController]
[Route("auth")]
public class AuthenticationController : ControllerBase
{
    private readonly ISender _mediator;

    public AuthenticationController(
        IMediator mediator)
    {
        _mediator = mediator;
    }

    [HttpPost("register")]
    public async Task<IActionResult> Register(RegisterRequest request)
    {
        var command = new RegisterCommand(request.FirstName, request.LastName, request.Email, request.Password);
        var authResult = await _mediator.Send(command);

        var authResponse = new SuccessResponse(
            authResult.User.Id,
            authResult.User.FirstName,
            authResult.User.LastName,
            authResult.User.Email,
            authResult.Token);
        return Ok(request);
    }

    [HttpPost("login")]
    public async Task<IActionResult> Login(LoginRequest request)
    {
        var query = new LoginQuery(request.Email, request.Password);
        var authResult = await _mediator.Send(query);
        var authResponse = new SuccessResponse(
            authResult.User.Id,
            authResult.User.FirstName,
            authResult.User.LastName,
            authResult.User.Email,
            authResult.Token);
        return Ok(request);
    }
}
```
</details>

### Swagger
**Web**
<details>
<summary>appsettings.Development</summary>
      
```json
"ApiSwaggerOptions": {
    "JsonRoute": "swagger/{documentName}/swagger.json",
    "Description": "Our API",
    "UIEndpoint":  "v1/swagger.json"
}
```
</details>
<details>
<summary>Options/ApiSwaggerOptions.cs</summary>
      
```csharp
public record ApiSwaggerOptions(
    string JsonRoute = null!,
    string UiEndpoint = null!,
    string Description = null!);
```
</details>
<details>
<summary>DependencyInjection.cs</summary>
      
```csharp
services.AddSwaggerGen(x =>
{
    x.SwaggerDoc("v1", new Microsoft.OpenApi.Models.OpenApiInfo
    {
        Title = "Api",
        Version = "v1"
    });
});
```
</details>
<details>
<summary>Program.cs</summary>
    
Добавить
    
```csharp
var swaggerOptions = new ApiSwaggerOptions();

builder.Configuration.GetSection(nameof(ApiSwaggerOptions))
    .Bind(swaggerOptions);

app.UseSwagger(option =>
{
    option.RouteTemplate = swaggerOptions.JsonRoute;
});

app.UseSwaggerUI(option =>
{
    option.SwaggerEndpoint(swaggerOptions.UiEndpoint, swaggerOptions.Description);
});
```
</details>
  
### JWT
