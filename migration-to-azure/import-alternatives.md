# Import alternatives

### 1. Alternative for importing data from a network share using an import connector

If data is imported from a network share, the alternative is to send the data to the existing import connector directly through a webservice call. You can either use your own middleware tooling to send data to the Ultimo import connector or use a cUrl command.

Actions:

* Create a \(powershell\) script to send data to the import connector.
* Execute the script immediately each time a file is added to the network share or add a scheduled task to run this script periodically, according to the current schedule on the import connector.

Note: make sure the IT infrastructure allows connection to the Ultimo webservices.

cUrl example:

```text
curl --data-binary @[FileName] -H "Content-type: text/xml;charset=utf-8" "[Import connector Url]"
curl --data-binary @C:\temp\importdata.csv -H "Content-type: text/xml;charset=utf-8" "https://customer.ultimo.net/Webservices/Connector.ashx?username=US0000003&password=xxx&action=import&importName=_CSVIMPORT"
```



### 2. Alternative for importing data/files from a network share using a scheduled workflow

If files are imported from a network share periodically with the workflow scheduler, the alternative is to send these files to the Ultimo REST API instead. The REST API can save these files in the Ultimo directory that is imported by the workflow. 

Actions:

* Create a \(powershell\) script to send data to the REST API.
* Execute the script immediately each time a file is added to the network share or add a scheduled task to run this script periodically

Note: make sure the IT infrastructure allows connection to the Ultimo webservices.

cUrl example:

```text
curl -F "File=@[FileName]" -F "FileSystemPath=[FileServiceData directory to save files]" -H "Apikey: [API Key]" -H "Applicationelementid: [Workflow GUID]" "[REST API Url]"
curl -F "File=@C:\temp\Document.docx" -F "FileSystemPath=ImportDirectory" -H "Apikey: xxx" -H "Applicationelementid: E7870B3EFDD5416BD7BE2C8AA660DDB8" "https://customer.ultimo.net/api/v1/action/_REST_UploadFile"
```

