# Log4Net .NET Core Console App Quickstart

Getting Log4Net working with a .NET Core console app is simple. You may have found guides for getting Log4Net working on a .NET Framework project. The setup is similar but requires a few additional steps. This quick start assumes you are using Visual Studio 2019.

## Installing  Log4Net
This process is the same for installing any other package using NuGet. Either the Package Manager Console or the Manage NuGet Packages GUI can be used.

### Package Manger Console
Open the Package Manager Console and run the following command.

`Install-Package log4net -Version 2.0.8`

### NuGet Packages GUI
In solution explorer, right click on your project and select 'Manage NuGet Packages'.
![Shows pop-up menu when project is right clicked.](projectRightClick.png?raw=true)

In the Browse section, search for 'Log4Net', select the latest version and press install.
![Shows NuGet Browser with Log4Net selected](projectNuGetShowingLog4Net.png)

## Creating Config File
We are going to create a configuration file that will make log4net log all levels off logs to the console appender. You can learn more about levels and different types of appenders [here](https://www.codeproject.com/Articles/140911/log-net-Tutorial).

In the root of your project create a new file called `log4net.config`. This file should be completely empty.

Add the following code to the file. Below I have explained what this does.
```
<?xml version ="1.0" encoding="utf-8"?>
<configuration>
  <log4net>
    <root>
      <level value = "ALL"/>
      <appender-ref ref="Console"/>
    </root>
    <appender name ="Console" type="log4net.Appender.ConsoleAppender">
      <layout type ="log4net.Layout.PatternLayout">
        <conversionPattern value="%-5p %d{hh:mm:ss} %message%newline" />
      </layout>
    </appender>
  </log4net>
</configuration>
```

Within the `<log4net>` tag there are two important sections, `<root>` & `<appender>`.

### `<root>`
This is where we can define what level of logs should be displayed and what appender to use.

You will notice that we have set the value of the level tag to 'ALL'. This means that all logs will be displayed.  This value can be changed to one of 7 values which will display varying levels of logs. These levels are:
- ALL
- DEBUG
- INFO
- WARN
- ERROR
- FATAL
- OFF

The next tag you will see if the `<appender-ref/>` tag. This tag has the attribute `ref` which we have set to `"Console"`. This refers to the name of the appender we have defined in the `<appender>` tag.

### `<appender>`
This tag has 2 attributes, `name` and `type`.

We have set the value of `name` to `"Console"`. Notice this is the same as the value of `ref` in the `<root>` tag.

The value of `type` has been set to `log4net.Appener.ConsoleAppender`. This means that we want to use the Console Appender. There are many different Appenders including ColorConsole, which allows for the console to display the log in different colors based on the its level. If you want to log to a file, research the RollingFileAppender.

## Calling configuration file in code.
Ensure that you are refrencing log4net. To do this include the following using statements.
```
using log4net;
using log4net.config;
```
In your projects `program.cs` file, add the following code.
```
var logRepository = LogManager.GetRepository(Assembly.GetEntryAssembly());
XmlConfigurator.Configure(logRepository, new FileInfo("log4net.config"));
```

## Copy config File
Remember to ensure that the config file we created earlier is being copied to the bin folder. To do this open up your projects `.csproj` file and enter the following code in the `<ItemGroup>` block.
```
<None Update="log4net.config">
      <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </None>
```

## Start Logging
We are almost at the end. In your `program.cs` file add the following line.
```
var _log4net = log4net.LogManager.GetLogger(typeof(Program));
```

We can now start logging! To write an info log see the following code
```
_log4net.Info("Hello Logging World");
```
This will display "Hello Logging World" to the console when the application is run!

## Next Steps

You are now able to start logging in your project. There are a wide range of different options for logging within Log4Net which are defiantly worth researching. This is only the beginning!
