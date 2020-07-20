---
layout: post
post_identifier: 2020-07-20-Try-And-Use-IdentityServer4-With-Blazor
title:  "Try And Use IdentityServer4 With Blazor"
date:   2020-07-09 16:02:24 +0100
categories: Tutorial
excerpt: loremNostrud est anim Lorem ea pariatur anim consequat sunt laborum pariatur et et
---

{% assign post_identifier = page.post_identifier %}


## Description/Introduction
Try And Use IdentityServer4 With Blazor

## References
* [GitHub](https://github.com/Xeppit/WpfIdentity)
* [IdentityModel.OidcClient.Samples](https://github.com/IdentityModel/IdentityModel.OidcClient.Samples/tree/main/WpfWebView/WpfWebView)
* [IdentityServer4.Demo](https://github.com/IdentityServer/IdentityServer4.Demo/blob/8f68a5258b81741847ae06c02f65423d67147cf5/src/IdentityServer4Demo/Config.cs#L81)

* [Possible Token Refresh Option](https://stackoverflow.com/questions/41741982/identityserver4-using-refresh-tokens-after-following-the-quickstart-for-hybrid)


## Goals
* Set up identity server
* Set up WebApi
* Set up Wpf application able to authenticate with identity server and retrive a token which includes User information.
* Make a call to the Api using the credentials

## Perquisites
* Dotnet SDK

## Creating Project Structure

{% include postimage.html 
postname=post_identifier
imageName="1-1.png" 
caption="Project Structure" %}

### Using the console create the project structure
```console
mkdir WpfIdentity
cd WpfIdentity
dotnet new sln -n WpfIdentity
dotnet new wpf -n WpfIdentity.Wpf
dotnet new is4aspid -n WpfIdentity.IdentityServer
dotnet new webapi -n WpfIdentity.Api
```
### Add the projects to the solution

```console
dotnet sln add WpfIdentity.Wpf
dotnet sln add WpfIdentity.IdentityServer
dotnet sln add WpfIdentity.Api
```
### Add To Source Control
Add the solution to to github using visual studio

## Configuring the Project

Https Port: 5000 -MicroserviceArchitecture.IdentityServer

Https Port: 5001 -MicroserviceArchitecture.Api

### MicroserviceArchitecture.IdentityServer/Properties/LaunchSettings.json
```json
{
  "profiles": {
    "SelfHost": {
      "commandName": "Project",
      "launchBrowser": false,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "https://localhost:5000"
    }
  }
}
```
### MicroserviceArchitecture.Api/Properties/LaunchSettings.json
```json
{
  "$schema": "http://json.schemastore.org/launchsettings.json",
  "profiles": {
    "MicroserviceArchitecture.Api": {
      "commandName": "Project",
      "applicationUrl": "https://localhost:5001",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```
## Configure IdentityServer
### MicroserviceArchitecture.IdentityServer/CustomIdentityResourceRoleClaim.cs
Creat a new class

```csharp
using IdentityModel;
using IdentityServer4.Models;

namespace MicroserviceArchitecture.IdentityServer
{
    public class CustomIdentityResourcesProfile : IdentityResources.Profile
    {
        public CustomIdentityResourcesProfile()
        {
            this.UserClaims.Add(JwtClaimTypes.Role);
        }
    }
}
```

### MicroserviceArchitecture.IdentityServer/Config.cs
```csharp
public static IEnumerable<IdentityResource> IdentityResources =>
    new IdentityResource[]
    {
        new IdentityResources.OpenId(),
        //new IdentityResources.Profile(),
        //overridden below
        new CustomIdentityResourcesProfile(), // New Line
        new IdentityResources.Email(),       
    };
```

```csharp
public static IEnumerable<ApiScope> ApiScopes =>
    new ApiScope[]
    {
        new ApiScope("api", "The Api", new[] { JwtClaimTypes.Role }), // Modified Line
    };
```

```csharp
public static IEnumerable<Client> Clients =>
    // New Client
    new Client[]
    {
        // interactive client using code flow + pkce
        new Client
        {
            ClientId = "wpf",
            AllowedGrantTypes = GrantTypes.Code,
            AllowedCorsOrigins = {"https://localhost:5001"},
            RequirePkce = true,
            RequireClientSecret = false,
            RedirectUris = {"https://notused"},
            PostLogoutRedirectUris = {"https://notused"},
            AllowedScopes = {"openid", "profile", "email", "api"},
            AllowOfflineAccess = true,
            RefreshTokenUsage = TokenUsage.ReUse
        }
    };
```

### MicroserviceArchitecture.IdentityServer/SeedData.cs
The database need to be re-seeded after this modification
```csharp
if (alice == null)
{
    alice = new ApplicationUser
    {
        UserName = "alice",
        Email = "AliceSmith@email.com",
        EmailConfirmed = true,
    };
    var result = userMgr.CreateAsync(alice, "Pass123$").Result;
    if (!result.Succeeded)
    {
        throw new Exception(result.Errors.First().Description);
    }
    result = userMgr.AddClaimsAsync(alice, new Claim[]{
        new Claim(JwtClaimTypes.Name, "Alice Smith"),
        new Claim(JwtClaimTypes.GivenName, "Alice"),
        new Claim(JwtClaimTypes.FamilyName, "Smith"),
        new Claim(JwtClaimTypes.WebSite, "http://alice.com"),
        new Claim(JwtClaimTypes.Role, "Admin") // New Line
    }).Result;
    if (!result.Succeeded)
    {
        throw new Exception(result.Errors.First().Description);
    }
    Log.Debug("alice created");
}
else
{
    Log.Debug("alice already exists");
}
```

### MicroserviceArchitecture.IdentityServer/Quickstart/Account/AccountOptions.cs
Redirect back to blazor after logout

```csharp
public static bool ShowLogoutPrompt = false;                    // Modified Line
public static bool AutomaticRedirectAfterSignOut = true;        // Modified Line
```

## Configering the Api
Using nuget manager add "Microsoft.AspNetCore.Authentication.JwtBearer" to MicroserviceArchitecture.Api

### MicroserviceArchitecture.Api/Startup.cs
```csharp
services.AddAuthentication("Bearer")
    .AddJwtBearer("Bearer", options =>
    {
        options.Authority = "https://localhost:5000";
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateAudience = false
        };
    });
```

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.UseCors(config =>
    {
        config.AllowAnyOrigin();
        config.AllowAnyMethod();
        config.AllowAnyHeader();
    });

    app.UseHttpsRedirection();
    app.UseRouting();
    // New Line
    app.UseAuthentication();
    app.UseAuthorization();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

### MicroserviceArchitecture.Api/Controllers/WeatherForecastController.cs
```csharp
//New Line
[Authorize]
[ApiController]
[Route("[controller]")]
```

## Configering the Wpf
### WpfIdentity.Wpf.csproj
```json
<Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>netcoreapp3.1</TargetFramework>
    <UseWPF>true</UseWPF>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="IdentityModel" Version="4.3.1" />
    <PackageReference Include="IdentityModel.OidcClient" Version="3.1.2" />
  </ItemGroup>

</Project>
```

### MainWindow.xaml
```xaml
<TextBlock x:Name="Message"
           Text="Authenticating..."
           HorizontalAlignment="Center"
           VerticalAlignment="Center"
           FontSize="24"
           FontFamily="Segoe UI Light, San Serif"
           TextWrapping="Wrap"/>
```
### MainWindow.xaml.cs
```csharp
public partial class MainWindow : Window
{
    private OidcClient _oidcClient = null;
    public MainWindow()
    {
        InitializeComponent();
        Loaded += Start;
    }
    public async void Start(object sender, RoutedEventArgs e)
    {
        var options = new OidcClientOptions()
        {
            Authority = "https://localhost:5000",
            ClientId = "wpf",
            Scope = "openid profile email api",
            RedirectUri = "https://notused",
            Browser = new WpfEmbeddedBrowser()
        };
        _oidcClient = new OidcClient(options);
        LoginResult result;
        try
        {
            result = await _oidcClient.LoginAsync();
        }
        catch (Exception ex)
        {
            Message.Text = $"Unexpected Error: {ex.Message}";
            return;
        }
        if (result.IsError)
        {
            Message.Text = result.Error == "UserCancel" ? "The sign-in window was closed before authorization was completed." : result.Error;
        }
        else
        {
            var Http = new HttpClient();
            Http.SetBearerToken(result.AccessToken);
            var response = await Http.GetAsync("https://localhost:5001/WeatherForecast");
            if (!response.IsSuccessStatusCode) throw new Exception(response.StatusCode.ToString());
            var apiResult = JsonConvert.DeserializeObject<List<WeatherForecast>>(await response.Content.ReadAsStringAsync());
            var name = result.User.Identity.Name;
            Message.Text = $"Hello {name}";
        }
    }
}
public class WeatherForecast
{
    public DateTime Date { get; set; }
    public int TemperatureC { get; set; }
    public string Summary { get; set; }
    public int TemperatureF { get; set; }// Modified Line
}
```
### WpfEmbeddedBrowser.cs
```csharp
class WpfEmbeddedBrowser : IBrowser
{
    private BrowserOptions _options = null;
    public WpfEmbeddedBrowser()
    {
    }
    public async Task<BrowserResult> InvokeAsync(BrowserOptions options, CancellationToken cancellationToken = default)
    {
        _options = options;
        var window = new Window()
        {
            Width = 900,
            Height = 625,
            Title = "IdentityServer Demo Login"
        };
        // Note: Unfortunately, WebBrowser is very limited and does not give sufficient information for 
        //   robust error handling. The alternative is to use a system browser or third party embedded
        //   library (which tend to balloon the size of your application and are complicated).
        var webBrowser = new WebBrowser();
        var signal = new SemaphoreSlim(0, 1);
        var result = new BrowserResult()
        {
            ResultType = BrowserResultType.UserCancel
        };
        webBrowser.Navigating += (s, e) =>
        {
            if (BrowserIsNavigatingToRedirectUri(e.Uri))
            {
                e.Cancel = true;
                result = new BrowserResult()
                {
                    ResultType = BrowserResultType.Success,
                    Response = e.Uri.AbsoluteUri
                };
                signal.Release();
                window.Close();
            }
        };
        window.Closing += (s, e) =>
        {
            signal.Release();
        };
        window.Content = webBrowser;
        window.Show();
        webBrowser.Source = new Uri(_options.StartUrl);
        await signal.WaitAsync();
        return result;
    }
    private bool BrowserIsNavigatingToRedirectUri(Uri uri)
    {
        return uri.AbsoluteUri.StartsWith(_options.EndUrl);
    }
}
```