# Export alternatives

### 1. Alternative for exporting data to a network share using an export connector

#### Export data periodically

If data is exported to a network share periodically, the alternative is to retrieve data from the existing export connector directly through a webservice call and save the result as a file. You can either use your own middleware tooling to get the data from the Ultimo export connector or use a cUrl command.

Actions:

* Create a \(powershell\) script to get and save data from the export connector.
* Add a scheduled task to run this script periodically, according to the current schedule on the export connector.

Note: make sure the IT infrastructure allows connection to the Ultimo webservices.

cUrl example:

```text
curl -o [OutputDirectory\FileName] -H "Content-type: text/xml;charset=utf-8" "[Export connector Url]"
curl -o C:\temp\outputdata.csv -H "Content-type: text/xml;charset=utf-8" "https://customer.ultimo.net/Webservices/Connector.ashx?username=US0000003&password=xxx&action=export&exportName=_CSVEXPORT"
```

#### Export after a specific action in Ultimo

If data is exported to a network share after an event in Ultimo has occurred, for example activating a record, an alternative is to send these files to a mailbox and configure the mailbox to save received files on the network share.

Actions:

* Configure a specific mailbox to receive these emails
* Configure the mailbox to save attachments of received emails to the network share

### 2. Alternative for exporting data/files to a network share using a scheduled workflow

If files are exported to a network share periodically with the workflow scheduler, the alternative is to  change the workflow so that files are send as an email attachment as described in the previous chapter.

Actions:

* Configure a specific mailbox to receive these emails
* Configure the mailbox to save attachments of received emails to the network share

