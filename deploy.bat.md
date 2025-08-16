# Publish, Zip and Deploy
The following code can be used to publish your .NET app to a Ubuntu Linux VPS.

```
@echo off

set "SRC=Your-published-folder\publish"
set "DEST=data.zip"
set "REMOTE=user@0.0.0.0"
set "REMDIR=/var/www/your-destination-folder"
set "SRVNAME=name.service"

echo Publishing the project using 'FolderProfile' ...
dotnet publish -c Release /p:PublishProfile=FolderProfile

echo ----------------------------------------------------
echo Removing unwanted files ...
del %SRC%\appsettings.Development.json
del %SRC%\*.pdb

echo ----------------------------------------------------
echo Compressing and copying files to server ...
del %DEST%
powershell -noprofile -Command "Compress-Archive -Path '%SRC%\*' -DestinationPath '%DEST%'"
echo Copying files to server ...
scp -r .\%DEST% %REMOTE%:%REMDIR%/
ssh %REMOTE% "unzip -o %REMDIR%/%DEST% -d %REMDIR%/"
ssh %REMOTE% "rm %REMDIR%/%DEST%"

echo ----------------------------------------------------
echo Restarting service ...
ssh %REMOTE% "systemctl restart %SRVNAME%"

echo ----------------------------------------------------
echo Done.
```

## Minimum Requirements
1. .NET SDK and your solution.
2. FolderProfile created once with appropriate options. So when you publish it uses to publish.
3. Powershell on your Windows machine (10 or 11).
4. Linux VPS setup SSH profile to autologin.
5. NGINX conf and service created to run.
6. Unzip application on Linux.
