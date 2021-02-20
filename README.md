# invoice
Console app that create invoice based on event feed

# Assumptions:
1. Will only run on Windows OS.
2. Invoice-dir is a network drive that has auto replication for 
3. Event Id is always incrementing by 1, and we can treat this number at the sequence of event creation.

# To improve
1. Store afterEventId on a network storage, so a crash won’t mean restarting from 0.
2. Make it compatible for multiple hosting.

This apps will take two parameters at startup:
1. —-feed-url
2. —-afterEventId

Eg: EventFeed.exe —-feed-url=https://api.com —-invoice-dir C:\Users\abc
