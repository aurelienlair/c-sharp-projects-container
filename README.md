# C# Docker projects container

## Description
the following project allows to run C# applications on a Docker container with the proper ecosystem.
Dotnet or whatever other dependency *will not* to be installed on your host since you're going to develop [inside](https://code.visualstudio.com/docs/remote/containers) the container.

## Installation
Within your Dockfile you can find the set-up for your container. Basically it will install the .NET Core [SDK](https://dotnet.microsoft.com/download). You can change the version but make sure you install at least version 2.2.
It is based on Debian 9 (stretch) for more info just click [here](https://hub.docker.com/_/microsoft-dotnet-core-sdk/).
The name of the Docker service is *c-sharp*.

First of all copy the docker-compose-override template:
```shell
$ cp docker-compose.override.yml.dist docker-compose.override.yml
```
and update `docker-compose.override.yml`:

- `${HOME}/c-sharp-projects` should be replaced with the path of the sources of your C# projects.
- 

Once the set-up ready you can directly run the docker-compose build from Visual Studio (please ensure you have the [Visual Studio Code Remote - Containers](https://code.visualstudio.com/docs/remote/containers) extension first) by running
```shell
$ code . --verbose
```
It will open Visual Studio then it should automatically detect your devcontainer.json  and should propose you to re-open it in a container:
![remote containers](./img/dev-container-reopen-prompt.png?display=inline-block)

If not you should be able to do it by running:
```
F1 > Remote-Containers: Add Development Container Configuration Files
```

- pre-install scripts
if needed you can customize your Docker environment by adding your custom shell scripts in the `./scripts` directory. In that directory you can configure 2 scripts.
`pre_install.sh` which will be ran _before_ any other commands just when the container is ready while `post_install.sh` will be ran _after_ everything is properly installed.

## Code style
The code style is enforced by `.editorconfig`.
Most editor's language servers implement support for it, but you can use the `dotnet-format` tool to automatically reformat the code around the style.
You can install it by running 
```shell
$ docker-compose exec c-sharp dotnet tool install -g dotnet-format
```
from Visual studio's terminal of  or in a classic terminal (see below) where you are connected to the remote container 
```shell
$ docker-compose exec c-sharp bash
$ dotnet tool install -g dotnet-format
```
## Tools

[Visual Studio Code](https://code.visualstudio.com/download) is the recommended editor when programming in C#.
Recommended extensions for VS Code are defined in `.vscode/extensions.json`.

## Tests

This project uses [xUnit](https://xunit.github.io/) for unit tests, [moq](https://github.com/moq/moq4) for mocking.
To run the tests run
```shell
$ cd ~/workspace
$ dotnet test
```

## Run dotnet commands from a classic shell terminal

By default this project will be installed with a container called `c-sharp-container`. If you use Visual Studio build-in terminal you shouldn't really care about it but if you prefer to use your classic terminal you can do it by running:
```shell
$ docker exec -it c-sharp-container dotnet build exercism/hello-world/HelloWorld.csproj
Microsoft (R) Build Engine version 16.1.76+g14b0a930a7 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 30.02 ms for /workspace/exercism/hello-world/HelloWorld.csproj.
  HelloWorld -> /workspace/exercism/hello-world/bin/Debug/netcoreapp2.1/HelloWorld.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:01.65

```

Have fun!