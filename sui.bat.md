# Blazor Wasm + simple/ui
The following is the batch programming code which upon execution will create a .NET solution with empty Blazor Wasm project, add it to solution, add package `Sysinfocus.AspNetCore.Components` and wire-up the required code so you are ready to start using the **Simple/ui** library.

```
@echo off

if "%~1"=="" (
    echo Error: No solution name provided.
    echo Usage: sui.bat [solution-name]
    echo.
    exit /b 1
)

dotnet new sln -n %1 || (
    echo Error: Solution creation failed.
    echo.
    exit /b 1
)

dotnet new blazorwasm --empty -n %1.App || (
    echo Error: Project creation failed.
    echo.
    exit /b 1
)

dotnet sln add ./%1.App/
dotnet add ./%1.App/ package Sysinfocus.AspNetCore.Components

powershell -NoProfile -Command "(Get-Content './%1.App/wwwroot/css/app.css') -replace '.valid.modified', ('*{margin:0;padding:0;box-sizing:border-box}' + [System.Environment]::NewLine + '.container{max-width:1200px;margin:auto;padding:1rem}' + [System.Environment]::NewLine + '' + [System.Environment]::NewLine + '.valid.modified') | Set-Content './%1.App/wwwroot/css/app.css'"

powershell -NoProfile -Command "(Get-Content './%1.App/wwwroot/index.html') -replace '</head>', ([char]9 + '<link rel=""""stylesheet"""" href=""""_content/Sysinfocus.AspNetCore.Components/styles.css"""" />' + [System.Environment]::NewLine + [char]9 + '<link rel=""""stylesheet"""" href=""""_content/Sysinfocus.AspNetCore.Components/Sysinfocus.AspNetCore.Components.bundle.scp.css"""" />' + [System.Environment]::NewLine + '</head>') | Set-Content './%1.App/wwwroot/index.html'"

powershell -NoProfile -Command "(Get-Content './%1.App/_Imports.razor') -replace '@using %1.App.Layout', ('@using %1.App.Layout' + [System.Environment]::NewLine + '@using Sysinfocus.AspNetCore.Components' + [System.Environment]::NewLine + '@inject BrowserExtensions browserExtensions' + [System.Environment]::NewLine + '@inject Initialization initialization' + [System.Environment]::NewLine + '@inject StateManager stateManager') | Set-Content './%1.App/_Imports.razor'"

powershell -NoProfile -Command "(Get-Content './%1.App/Program.cs') -replace 'using %1.App;', ('using %1.App;' + [System.Environment]::NewLine + 'using Sysinfocus.AspNetCore.Components;') | Set-Content './%1.App/Program.cs'"

powershell -NoProfile -Command "(Get-Content './%1.App/Program.cs') -replace 'await builder.Build', ('builder.Services.AddSysinfocus();' + [System.Environment]::NewLine + [System.Environment]::NewLine + 'await builder.Build') | Set-Content './%1.App/Program.cs'"

powershell -NoProfile -Command "(Get-Content './%1.App/App.razor') -replace '<FocusOnNavigate RouteData=""""@routeData"""" Selector=""""h1"""" />', '' | Set-Content './%1.App/App.razor'"

powershell -NoProfile -Command "(Get-Content './%1.App/Layout/MainLayout.razor') -replace '@Body', ('<div class=""""container"""" @onclick=""""@initialization.HandleMainLayoutClickEvent"""">' + [System.Environment]::NewLine + [char]9 + '@Body' + [System.Environment]::NewLine + '</div>' + [System.Environment]::NewLine + [System.Environment]::NewLine + '@code {' + [System.Environment]::NewLine + [char]9 + 'protected override async Task OnAfterRenderAsync(bool firstRender) ' + [System.Environment]::NewLine + [char]9 + '{' + [System.Environment]::NewLine + [char]9 + [char]9 + 'if (firstRender) await initialization.InitializeTheme();' + [System.Environment]::NewLine + [char]9 + '}' + [System.Environment]::NewLine + '}') | Set-Content './%1.App/Layout/MainLayout.razor'"
```


## How to use
1. Create a .bat file for eg: `sui.bat` with the above code in a folder where you want to create the solution and project.
2. Run `sui.bat Test` - Test is the solution name and the app will be Test.App
3. Once created, you can run project with the code `dotnet run --project ./Test.App/`
4. That's it. You now have a complete app with component library wired-up.

## Minimum Requirements
1. .NET SDK (version 9.0 and above recommended)
2. Windows 10 and above
3. Powershell (should be available by default)
