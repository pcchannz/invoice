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
2. Using Serilog instead of Microsoft for ease of switching between 3rd party logging without code change. 
3. Automatically fallback to user input for new feed-url if the existing one failing to response for 3 consecutive calls.
4. Run without options will prompt user input.
5. Directory and url validation before start pooling.
6. Using DI to inject services as singleton to main program.
7. Including a minimal xunit unit test project
8. Implemented with event based notification to invoice generation
9. Tested with a mockaroo API! https://my.api.mockaroo.com/events.json?key=fda01bf0

# Assumptions:
1. Server has dotnet 5.0 SDK installed.The zip file doesn't include exe or binary. Just build and run on your favourite env ~
2. Invoice-dir is a network drive that has auto replication for backup and redundancy.
3. Event Id is always increment by 1. We can treat this number as the sequence of event creation.
4. No need to manage docs versioning, storage has versioning capability / can always recreate from event API.
5. Invoice_update always contains full detail, so no need to check for delta, directly replace current version.
6. Invoice_create will be upsert. Assuming storage has versioning capability, no need to worry skip create if exists, just replace it and let storage do the versioning.
7. Logs are safe to store locally as it has same network drive capability as invoice-dir.

# Shortcuts:
1. Use 3rd party libraries such as PdfShareCore, Serilog to speed up development process.
2. Build unit test project structure but have not fully covered test cases.

# To improve
1. Store afterEventId on a network storage, so a crash won’t means restarting from 0.
2. Configure CI/CD to automatically test and deploy.
3. Support more types of invoice-dir such as s3, ftp.
4. Redesign to process simultaneously across multiple instances without double processing to support horizontal scaling
5. Improve test scenarios and code coverage.
6. Allow more command line options to override appsettings.json.
7. Improve PDF formatting.
8. Create a mock server API instead of using mockaroo.
9. Benchmark the efficiency.
10. Try hook up with cloud services such as SQS and SNS.
11. Logging to cloudwatch and add x-ray for each processing step.

# To test
This apps will take two parameters at startup:
- --feed-url
- --invoice-dir

1. unzip EventFeed.zip
2. cd EventFeed
3. dotnet run --feed-url=https://my.api.mockaroo.com/events.json?key=fda01bf0 --invoice-dir=C:\Users\abc
