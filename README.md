##Event Interface
---
The EventInterface.exe is a MS .NET command line executable that can be called to create an Incident through AMT’s Service Management Integration.  It accepts values used to create the Incident one of two ways.

1. XML Template File - This XML file contains the static values that are used to set the Incident values.
2. Name/Value Command Line Parameters - Each XML Template File element can be overwritten by a corresponding command line parameter.

Each of these is described in more detail later in this document.  Examples demonstrate the application's functionality. 

The standard usage for this application is to enable automated Incident creation as the result of some event.  The event can be network or hardware related and signals the creation of an Incident for further handling. Most monitoring systems support passing event information to a command line application for further processing.

The resulting Incident Number is returned synchronously in the response and written to the Event Log.  

The integration can be configured to send an asynchronous response to a listening URL web service, XML POST or SMTP message at your request.  


###Setup
---
1. Run Setup.exe and follow the prompts of the installation wizard.
2. Update EventInterfaceFile.xml with relevant values for your Companies environment.
3. Submit a request through Technology Support:  https://technologysupport.accenture.com/ 

  1. You will need to Submit a Request for Application Support for ITSM:
   
  2. Request for AMT Service Management XML Document Integration Access
    * The Service Management Company Name you are integrating with.
    * Describe your integration requirements.  
  3. You require the following:
    * Integration ID / PW and Target Name
4. Update EventInterface.exe.config ID / PW and Target Name


###Logging
---
The results for each execution are written to the system event log.  The command line 
parameters template file and results are captured.

Event Log Example:
```
	Event Interface

	Command Line Parameters:
	ExtRefNo=BPPM-TEST0000100-140
	Description=TEST 05-100*1300
	DetailedDescription=VOOM-VOOM SIMCA BO<AEA MIA LA TOCA HOLOA BOIN TENTA

	Event Interface File:
	C:\temp\EventInterfaceFile.xml

	Integration Response:
	Status: OK
	ID: INC700025856528
	Customer Reference: BPPM-TEST0000100-140
	Details:   Client Name: Accenture System ID: ACBPPM

	Return Code= 0 (Success)
```


###Requirements
---
The application is distributed with 4 files.
  - EventInterface.exe - Executable.
  - EventInterface.exe.config - The Event Interface integration details are stored here.  
  - EventInterfaceFile.xml - This is the default XML Template File.  

####EventInterface.exe.config
You will need to update this file with the URL, ID, PW and TargetID for your integration.  
* URL – Integration URL requires ID/PW for authentication.
* ID / PW – The ID/PW used to authenticate with the Integration URL.
* TargetID – The unique Target Name for your integration instance.  Target ID can be passed in the *template* `xml` file as a tag, `<TargetID>[Target ID]</TargetID>`.

####EventInterfaceFile.xml
This file can be used to create multiple variations of the default values used to 
create the Incident.  If other files are used, pass the file name as a command line 
parameter.  

Values in the XML file are overwritten by those passed through the command line.

#####Environment Information
Support for WIN32 executable such as Windows Server 2003-2013, XP, Vista, Win 7 & 8 etc...


###XML Template File
---
The XML Template File name can be passed as a command line parameter.  If the file is 
not passed, the application will default to "EventInterfaceFile.xml" in the current 
application working directory.
 
The XML Template File name can be passed as the EventInterfaceFile parameter in the form 
of a full path and file name.  

   * For example:
     `C:\temp\>EventInterface.exe EventInterfaceFile="c:\temp\Incident1.xml" `

	

###Name/Value Command Line Parameters
---
Command line parameters are passed as Name/Value pares separated by "=".  Each of the Values should be enclosed by quotes, "".  The list of available values is shown under Incident Parameters.

Each of the XML Template elements can be set by passing a command line parameter.  For Example, `<Description></Description>` can be overwritten by passing `Description="Hello World!"`. 

###Examples
---

Passing the value "show" will write the application output to the console.

Examples:
     `C:\temp\>\EventInterface.exe show Description="Description of the Event for the help desk here…" EventInterfaceFile="c:\temp\Incident1.xml"`


###Return Codes -
---
The results for each execution are written to the system event log.  The following exit codes are passed to the console.
```
Success = 0,
Error Reading XML = 1,
Invalid Filename = 2,
Error Parsing XML File = 3,
Error Initializing Parms = 4,
Error Calling WS = 5,
Error Creating Ticket = 6,
Unknown = 10 (see Event Log for details)
```
---


###Default Values
---
Set Default Customer, Classification and Assignment values in the template or via the command line.  Defaults are used when the values passes cannot be found in the system.  


###Incident Parameters -
---
Each XML Template File element can be overwritten by a corresponding command line parameter shown here.

ITSM style parameters:
```ExtRefNo
IncidentNumber
Status
StatusReason
Description
DetailedDescription
Priority
Responded
CustomerCompany
EmailAddress
DirectContactCompany
DirectContactEmail
DirectContactFirstName
DirectContactMidName
DirectContactLastName
DirectContactPhoneNum
DirectContactOrg
DirectContactDept
DirectContactSite
ClassificationCompany
ReportedDate
ReportedSource
SetRespondedDate
ServiceType
CatTier1
CatTier2
CatTier3
ProdCatTier1
ProdCatTier2
ProdCatTier3
ProdName
ProdModelVersion
AssignSuppCompany
AssignSuppOrg
AssignGroup
AssigneeFullName
AssigneeLoginName
CIName
DefaultCustEmail
DefaultImpact
DefaultUrgency
DefaultPriority
DefaultPriorityWeight
DefaultOppCat1
DefaultOppCat2
DefaultOppCat3
DefaultProdCat1
DefaultProdCat2
DefaultProdCat3
DefaultProdName
DefaultAssignedCo
DefaultAssignedOrg
DefaultAssignedGroup
DefaultAssigneeFN
DefaultAssigneeLogin
Resolution
ResolutionCatTier1
ResolutionCatTier2
ResolutionCatTier3
ResolutionCause
ResolutionMethod
ResolutionProdCatTier1
ResolutionProdCatTier2
ResolutionProdCatTier3
ResolutionProdName
Responded
```
Refer to the Event Interface mapping template *ITSMXML_Event_Interface_Map.xlsx* for a detail description of how to set these fields and to see how these values map to the fields in the Incident form.

