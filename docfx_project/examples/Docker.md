# Running Puppeteer-Sharp on Docker

_Contributors: [Darío Kondratiuk](https://www.hardkoded.com/)_

## Problem

You want to run Puppeteer-Sharp on Docker.

## Solution

We can create a Puppeteer-Sharp Based on the [example provided by Puppeteer](https://github.com/GoogleChrome/puppeteer/blob/master/docs/troubleshooting.md#running-puppeteer-in-docker)
Add [Serilog.Extensions.Logging.File](https://www.nuget.org/packages/Serilog.Extensions.Logging.File/) Nuget package.

First we need to create an `ILoggerFactory`

```js
private static ILoggerFactory GetLoggerFactory(string file)
{
    var factory = new LoggerFactory();
    var filter = new FilterLoggerSettings
    {
        { "Connection", LogLevel.Trace }
    };

    factory.WithFilter(filter).AddFile(file, LogLevel.Trace);

    return factory;
}
```

Now we can use `GetLoggerFactory` to inject a logger into Puppeteer.

```cs
using (var browser = await Puppeteer.LaunchAsync(browserOptions, GetLoggerFactory(fileName)))
{
    //Some code
}
```