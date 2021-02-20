# invoice
Console app that create invoice based on event feed

# Assumptions:
1. Will only run on Windows OS.
2. Invoice-dir is a network drive that has auto replication for backup and redundancy.
3. Event Id is always incrementing by 1, and we can treat this number at the sequence of event creation.
4. Don’t need to create versioning of docs, can always be recreated from event API.
5. Invoice_update always contains full details, so we don’t need to check for delta, and directly replace current version.
6. Invoice_create will be upsert.
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
