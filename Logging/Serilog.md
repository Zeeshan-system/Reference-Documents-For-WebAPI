# Logging with Serilog

## Installation

```bash
#Dowload the required packages in CLI
dotnet add package Serilog

#To log the files into a file 
dotnet add package Serilog.Sinks.File

#To Enrich with ClientInfo
dotnet add package Serilog.Enrichers.ClientInfo

#To Enrich with HTTPcontext
dotnet add package Serilog.Enrichers.AspnetcoreHttpcontext
```
## Setting up appsettings.json
* Enricher to Enrich Log Console with ClientIP data
```json
"Serilog": {
    "Using":  ["Serilog.Sinks.File","Serilog.Enrichers.ClientInfo", "Serilog.Enrichers.AspnetcoreHttpcontext"],
    "MinimumLevel": {
      "Default": "Debug",
      "Override": {
        "Default": "Information",
        "Microsoft": "Warning",
        "Microsoft.Hosting.Lifetime": "Information"
      }
    },
    "Enrich": [
      "WithClientIp",
      "WithClientAgent",
      "FromLogContext"
    ],
    "WriteTo": [
      {
        "Name": "Console"
      },
      {
        "Name": "File",
        "Args": {
          "path": "logs/api_log.txt",
          "outputTemplate": "{Timestamp:G} UTC - [{Level}] {ClientIp} {UserName} {Message}{NewLine:1}{Exception:1}"
        }
      }
    ]
  }
```

## Setting up Program.cs fle in .net 6
```csharp
using Serilog;
```
```csharp
//Logger setup
builder.Services.AddHttpContextAccessor();
builder.Logging.ClearProviders();
var logger = new LoggerConfiguration()
    .ReadFrom.Configuration(builder.Configuration)
    .CreateLogger();
builder.Logging.AddSerilog(logger);
```
```csharp
//DI Service for Serilog
builder.Services.AddSingleton(logger);
```

## Reference
[TestAPI](https://github.com/Zeeshan-system/Test-Web-API) Repository.
