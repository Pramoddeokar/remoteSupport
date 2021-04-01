# Telemetry

The Remote support app logs telemetry to [Azure Application Insights](https://azure.microsoft.com/en-us/services/monitor/). You can go to the respective Application Insights blade of the Azure App Services to view basic telemetry about your services, such as requests, failures, and dependency errors, custom events, traces etc.

The Remote support app integrates with Application Insights to gather bot activity analytics, as described [here](https://blog.botframework.com/2019/03/21/bot-analytics-behind-the-scenes/).

The app logs AadObjectId of user for tracing logs. The deployer should ensure that the solution meets their privacy/data retention requirements, and can choose to remove it if they wish.

The app logs following events:

`Activity`:
- Basic activity info: `ActivityId`, `ActivityType`, `Event Name`
- Basic user info: `FromID`

`UserActivity`:
- Basic activity info: `ActivityId`, `ActivityType`, `Event Name`
- Basic user info: `UserAadObjectId`
- Context of how it was invoked: `ConversationType`

`customEvents`:
- CRUD operations logging.

*Application Insights queries:*

- Get list of traces messages and count when bot is added to 1:1 chat in last 30 days.

```
traces
| where message contains "Bot added to 1:1 chat"
| where timestamp >= ago(30d) 
| summarize count() by message
```

- Number of times bot is added to team successfully in last 30 days.
  
```
traces
| where message contains "Bot added to team"
| where timestamp >= ago(30d) 
| summarize count() by message
```

- This query gives total number of requests escalated in last 30 days.

```
traces
| where message contains "Requests escalated."
| where timestamp >= ago(30d)
```

- This query gives number of requests Closed and Reopened in last 30 days.

```
traces
| where message contains "Received submit: action= closed/reopened"
| where timestamp >= ago(30d)
```

- This query gives average number of users creating requests.
```
traces
| where message contains "New request created."
| where timestamp >= ago(30d)
```

![trace_example](/Wiki/Images/trace_example.png)

*Application Insights Log Levels:*
- **Trace = 0** : Logs that contain the most detailed messages. These messages may contain sensitive application data. These messages are disabled by default and should never be enabled in a production environment.
- **Debug = 1** : Logs that are used for interactive investigation during development. These logs should primarily contain information useful for debugging and have no long-term value.
- **Information = 2** : Logs that track the general flow of the application. These logs should have long-term value.
- **Warning = 3** :Logs that highlight an abnormal or unexpected event in the application flow, but do not otherwise cause the application execution to stop
- **Error = 4** : Logs that highlight when the current flow of execution is stopped due to a failure. These should indicate a failure in the current activity, not an application-wide failure.
- **Critical = 5** :Logs that describe an unrecoverable application or system crash, or a catastrophic failure that requires immediate attention.
- **None = 6** : Not used for writing log messages. Specifies that a logging category should not write any messages.

If the Admin user wants to change Log Level, he/she has to go to Application Settings in the Configuration of the App Service and change the Log Level value for "ApplicationInsightsLogLevel" .
For e.g. 
"ApplicationInsightsLogLevel": "Information"

Below are the possible values of Log Level:  
1. Trace
2. Debug
3. Information
4. Warning
5. Error
6. Critical
7. None