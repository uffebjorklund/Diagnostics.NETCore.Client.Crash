# Diagnostics.NETCore.Client.Crash

Minimal repo to reproduce crash when building single file with Diagnostics.NETCore.Client installed

The Diagnostics.NETCore.Client package cant be installed when publishing to SingleFile

## Issue Tracker

https://github.com/dotnet/diagnostics/issues/1919

## Reproduce

Just build the project with `dotnet publish -r ubuntu-x64 /p:PublishSingleFile=true Diagnostics.NETCore.Client.Crash.csproj`

## Result

```
/usr/share/dotnet/sdk/5.0.101/Sdks/Microsoft.NET.Sdk/targets/Microsoft.NET.Publish.targets(1016,5): error MSB4018: The "GenerateBundle" task failed unexpectedly. [/home/uffe/Repositories/github/uffebjorklund/Diagnostics.NETCore.Client.Crash/Diagnostics.NETCore.Client.Crash.csproj]
/usr/share/dotnet/sdk/5.0.101/Sdks/Microsoft.NET.Sdk/targets/Microsoft.NET.Publish.targets(1016,5): error MSB4018: System.ArgumentException: Invalid input specification: Found multiple entries with the same BundleRelativePath [/home/uffe/Repositories/github/uffebjorklund/Diagnostics.NETCore.Client.Crash/Diagnostics.NETCore.Client.Crash.csproj]
/usr/share/dotnet/sdk/5.0.101/Sdks/Microsoft.NET.Sdk/targets/Microsoft.NET.Publish.targets(1016,5): error MSB4018:    at Microsoft.NET.HostModel.Bundle.Bundler.GenerateBundle(IReadOnlyList`1 fileSpecs) [/home/uffe/Repositories/github/uffebjorklund/Diagnostics.NETCore.Client.Crash/Diagnostics.NETCore.Client.Crash.csproj]
/usr/share/dotnet/sdk/5.0.101/Sdks/Microsoft.NET.Sdk/targets/Microsoft.NET.Publish.targets(1016,5): error MSB4018:    at Microsoft.NET.Build.Tasks.GenerateBundle.ExecuteCore() [/home/uffe/Repositories/github/uffebjorklund/Diagnostics.NETCore.Client.Crash/Diagnostics.NETCore.Client.Crash.csproj]
/usr/share/dotnet/sdk/5.0.101/Sdks/Microsoft.NET.Sdk/targets/Microsoft.NET.Publish.targets(1016,5): error MSB4018:    at Microsoft.NET.Build.Tasks.TaskBase.Execute() [/home/uffe/Repositories/github/uffebjorklund/Diagnostics.NETCore.Client.Crash/Diagnostics.NETCore.Client.Crash.csproj]
/usr/share/dotnet/sdk/5.0.101/Sdks/Microsoft.NET.Sdk/targets/Microsoft.NET.Publish.targets(1016,5): error MSB4018:    at Microsoft.Build.BackEnd.TaskExecutionHost.Microsoft.Build.BackEnd.ITaskExecutionHost.Execute() [/home/uffe/Repositories/github/uffebjorklund/Diagnostics.NETCore.Client.Crash/Diagnostics.NETCore.Client.Crash.csproj]
/usr/share/dotnet/sdk/5.0.101/Sdks/Microsoft.NET.Sdk/targets/Microsoft.NET.Publish.targets(1016,5): error MSB4018:    at Microsoft.Build.BackEnd.TaskBuilder.ExecuteInstantiatedTask(ITaskExecutionHost taskExecutionHost, TaskLoggingContext taskLoggingContext, TaskHost taskHost, ItemBucket bucket, TaskExecutionMode howToExecuteTask) [/home/uffe/Repositories/github/uffebjorklund/Diagnostics.NETCore.Client.Crash/Diagnostics.NETCore.Client.Crash.csproj]
```

## Details

 - If we remove `/p:PublishSingleFile=true` it will build just fine.
 - If we remove the packages
   ```
   <ItemGroup>
    <PackageReference Include="Microsoft.Diagnostics.NETCore.Client" Version="0.2.160202" />
    <PackageReference Include="Microsoft.Diagnostics.Tracing.TraceEvent" Version="2.0.64" />
   </ItemGroup>
   ```
   it will build just fine.
