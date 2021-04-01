The app uses the following data stores:

1. Azure Storage Account

   * [Table] Stores the ticket information created by end user and managed by SMEs.
   * [Table] Stores inforamtion of on call experts.
   * [Table] Stores card configuration details.
   * [Table] Stores the maximun ticket id.

2. Azure Search service list item index
   
   * Search service to query data related to tickets from Azure storage.

 All these resources are created in your Azure subscription. None are hosted directly by Microsoft.

# Storage account
## TicketDetail Table
The **TicketInfo** table stores data about the ticket information created by end user and managed by SMEs. Each row in the table has the following columns:

|Attribute | Comment|
-----------|---------
|PartitionKey|This represents the partition key of the azure storage table - Constant- [Ticket]|
|RowKey|Represents the unique ticket id of each row.|
|TicketId|Represents the unique ticket id of each row.|
|CreatedOn|Contains the date time of Ticket creation.|
|CreatedByUserPrincipalName|The email address of the end user who created ticket.|
|CreatedByObjectId|The AAD Object Id of user who created ticket.|
|RequesterName|The name of the requester.|
|LastModifiedOn|Last date time on which ticket was updated.|
|LastModifiedByName|Name of the user who last updated ticket.|
|LastModifiedByObjectId|The AAD Object Id of user who created ticket.|
|AssignedToName|The email address of the user to whom Ticket is assigned.|
|AssignedOn|Contains the date time on which Ticket is assigned to SME.|
|AssignedToObjectId|The AAD Object Id of user to whom ticket is assigned.|
|ClosedByName|The email address of the user who closed Ticket.|
|ClosedOn|The date time on which Ticket is closed.|
|ClosedByObjectId|The AAD Object Id of user who closed the ticket.|
|TicketStatus| value: 0 = Unassigned / 1 = Assigned/ 2 = Closed/ 3 = Withdrawn|
|Title|Ticket title provided by end user.|
|Description|The description text that is written by the end user.|
|Severity|Integer type: 0 = Normal / 1 = Urgent.|
|RequestType|The severity of the ticket Id.|
|IssueOccuredOn|User provided date on which issue was first noticed.|
|RequesterConversationId|The conversationId of the 1:1 chat between the end user and the Remote Support bot.|
|RequesterTicketActivityId|The activityId when the new Ticket adaptive card is posted in the personal scope.|
|SmeConversationId|The conversationId in the SME team channel at the time a new Ticket is created.|
|SmeTicketActivityId|The activityId when the new Ticket adaptive card is posted in the SME team channel.|
|AdditionalProperties|The ticket additional properties.|
|CardId|The new card Id.|

## OnCallSupportDetail table

|Attribute | Comment|
|-----------|---------
|Partition Key|This represents the partition key of the azure storage table - Constant - "OnCallSMEMetadata".|
|Row Key|On call support Id which is unique identifiers of the row.|
|OnCallSMEs|String of object id of on call experts.|
|ModifiedByName|The name of the SME user who recently updated on call experts list.|
|ModifiedByObjectId|The AAD Object id of the SME user who recently updated on call experts.|
|ModifiedOn|The date time on which on call experts list was updated.|

## CardConfiguration table
|Attribute | Comment|
|-----------|---------
|Partition Key|This represents the partition key of the azure storage table - Constant - "Card".|
|Row Key|Card Id as GUID to uniquely identifies the Card.|
|TeamId|Id of the team in which card configuration created.|
|TeamLink |URL of the expert team.|
|CreatedOn|Card creation date.|
|CreatedByUserPrincipalName|The user principal name (UPN) of the user that created the ticket.|
|CreatedByObjectId|The Azure Active Directory objectId of user who created ticket.
|CardTemplate|adaptive card items json properties.|

## TicketIdGenerator table
|Attribute | Comment|
|-----------|---------
|Partition Key|This represents the partition key of the azure storage table - Constant - "TicketIdGeneratorPartitionKey".|
|MaxTicketId|The ticket id of the created ticket.|
