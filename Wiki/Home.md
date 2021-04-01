 
# Remote Assistance

**Applicable scenarios:** Remote Assistance works really well for help desk scenarios. It also works well to provide quick support. Also provide centralized support to employees across the organization often distributed geographically

**Following are the features provided in Remote support**
An end-user interacting in Remote Assistance:

### End User Commands :

- *New Request:* An End User will be able to raise a new ticket using NewRequest Command. Once request is submitted, user will be able to withdraw,edit the same ticket also a card with request details is sent in the team which is configured as the SME team.

![New-request](/Wiki/Images/new-request.png)

![New-request-enduser](/Wiki/Images/new-request-submitted.png)

![New-request-SME](/Wiki/Images/new-request-SME-card.png)

### Messaging extension for end users:
End users will be able to search and query user’ own requests through the messaging extension (ME) in the personal scope This will be useful in cases of if they want to get details regarding a particular request or escalate an active request which was not acted upon for a long time. It will have tabs for 
 - *Active*: This tab will feature only those requests whose status is not Closed/Withdrawn 
 - *Closed* : This tab will feature only those requests whose status is not Closed/Withdrawn

![messaging-extension](/Wiki/Images/messaging-extension.png)

### Messaging extension for SME:
SME users will be able to search and query requests through the messaging extension (ME). It will have tabs for 
-  *Urgent:* This tab will feature only those requests whose severity is Urgent
- *Assigned:* This tab will feature all requests whose status is marked as Assigned.
- *Unassigned:* This tab will feature all requests whose status is marked as Unassigned.

### Expert team commands :

- *Expert List* : Using this command,Expert Team will be able to see and manage on call Expert Team. Members who are a part of on-call experts list will be added to group chats for requests that have been escalated by end users.

![groupchat](/Wiki/Images/messaging-extension.png)

### **Bot Commands**
The supported BOT commands for the app are described in this section.

| **Bot Command** |**Bot Response**|
|----------------|------------------------------|
|**New Request**|EndUser will be able to create a new request.|
|**Expert List**|Members of the expert’s team will be able to manage the on-call experts list using the command  “Expert List”.|

- [Solution overview](Solution-Overview)
	- [Data stores](Data-Stores)
	- [Cost estimate](Cost-Estimates)

- Deploying the app
	- [Deployment guide](Deployment-Guide)
	- [Troubleshooting](Troubleshooting)
