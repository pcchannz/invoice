# invoice
Console app that create invoice based on event feed

# Assumptions:
1. Require dotnet 5.0 SDK
2. Invoice-dir is a network drive that has auto replication for backup and redundancy.
3. Event Id is always increment by 1, and we can treat this number as the sequence of event creation, instead of using creation date.
4. No need to manage docs versioning, storage has versioning capability / can always recreate from event API.
5. Invoice_update always contains full detail, so no need to check for delta, directly replace current version.
6. Invoice_create will be upsert. Assuming storage has versioning capability, no need to worry skip create if exists, just replace it and let storage do the versioning.
7. Logs are safe to storage locally as it’s the same network drive with auto replication.

# To improve
1. Store afterEventId on a network storage, so a crash won’t mean restarting from 0.
2. Make it compatible for multiple hosting.
3. More invoice for option, eg: s3, ftp
4. Rearchitect to coordinate and work together with multiple instances to speed up processing.


This apps will take two parameters at startup:
1. —-feed-url
2. —-afterEventId

Eg: EventFeed.exe —-feed-url=https://api.com —-invoice-dir C:\Users\abc
