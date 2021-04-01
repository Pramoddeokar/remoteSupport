  TODO![architecture-overview](/Wiki/Images/architecture_overview.png)

The **Remote support** application has the following main components:

* **Remote support Bot**: 
Remote Support Bot provides all end users (internal users seeking help from a central team) and easy interface (bot) to:
  *  Submit requests
  * Edit/withdraw requests
  * Notify end users about the status of their request
  * Escalate to a group chat that connects them immediately with an expert

  All the requests raised or created by end users will get routed to a specific/central channel which allows the members of the channel an easy interface (a bot within their teams/channel) that:
  * Enables them to see in real-time all incoming requests with associated details
  * Ability to start an instant chat message with the requesters
  * Ability to receive and act upon an incoming Teams group chat from the remote requesters
  * Ability to manage incoming requests within the central team (lightweight ticketing)
  * Manage the list of experts who will be on-call to receive incoming Teams group chat requests
  * Allows experts to pick where the bot will post real-time notifications for support (default = general channel).

## Bot and Messaging Extension
The bot is built using the [Bot Framework SDK v4 for .NET](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0) and [ASP.NET Core 2.1](https://docs.microsoft.com/en-us/aspnet/core/?view=aspnetcore-2.1). The bot has a conversational interface in personal (1:1) scope for end-users and in team scope for the experts team. It also implements a messaging extension with [query commands](https://docs.microsoft.com/en-us/microsoftteams/platform/concepts/messaging-extensions/search-extensions), which the experts team can use to search for and share requests or knowledge base questions. It also implements messaging extension that end user can use to create ticket.