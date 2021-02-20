# Summary 
Console app that create invoice based on event feed

# Overview of design

1. Configurable via appsettings.json
```
{
  "PageSize" : 10,
  "AfterEventId" : 0,
  "MaxPageSize": 500, //DO NOT CHANGE THIS UNLESS YOU KNOW THE API MAX PAGE SIZE
  "PollingIntervalInSeconds": 5,
  "Serilog": {
    "Using": [ "Serilog.Sinks.Console", "Serilog.Sinks.File" ],
    "MinimumLevel": "Information",
    "WriteTo": [
      { "Name": "Console" },
      {
        "Name": "File",
        "Args": { "path": "Logs/log.txt" }
      }
    ],
    "Properties": {
      "Application": "EventFeed"
    }
  }
}
```
2. Use serilog logging instead of microsoft.extension.logging so it could extend to 3rd party logging without code change. 
3. Allow fallback to user input if feed-url is failing to response for 3 consecutive calls.
4. Run without options will prompt user input.
5. Will check for directory and url validity before start pooling.
6. Use DI to inject services as singleton to main program.
7. Include a minimal xunit unit test project
8. Implement event based notification to invoice generation
9. Test with a mockaroo API! https://my.api.mockaroo.com/events.json?key=fda01bf0

# Assumptions:
1. Require dotnet 5.0 SDK
2. Invoice-dir is a network drive that has auto replication for backup and redundancy.
3. Event Id is always increment by 1, and we can treat this number as the sequence of event creation, instead of using creation date.
4. No need to manage docs versioning, storage has versioning capability / can always recreate from event API.
5. Invoice_update always contains full detail, so no need to check for delta, directly replace current version.
6. Invoice_create will be upsert. Assuming storage has versioning capability, no need to worry skip create if exists, just replace it and let storage do the versioning.
7. Logs are safe to storage locally as it’s the same network drive with auto replication.

# Shortcuts:
1. Use 3rd party libraries such as PdfShareCore, Serilog to speed up development process.
2. Build unit test project structure but have not finished up the test.

# To improve
1. Store afterEventId on a network storage, so a crash won’t mean restarting from 0.
2. Make it compatible for multiple hosting.
3. More invoice for option, eg: s3, ftp.
4. Rearchitect to coordinate and work together with multiple instances to speed up processing.
5. Improve test scenarios and code coverage.
6. Allow more command line options to override appsettings.json.
7. Improve PDF formatting.
8. Create a mock server API instead of using mockaroo.


This apps will take two parameters at startup:
1. —-feed-url
2. —-invoice-dir

Eg: dotnet run —-feed-url=https://my.api.mockaroo.com/events.json?key=fda01bf0 —-invoice-dir C:\Users\abc
