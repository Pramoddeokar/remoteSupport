## Scenario 1:
Customize the app with "Subscribe" button.

**Suggested Solution:** Currently only one SME team can receive ticket details. With subscribe option other team can also receive ticket information.

**Code Changes:**

- Allow multiple team Id to be configured.
 
**Pros:** Multiple SME team can handle ticket requests.

## Scenario 2:
Export ticket request data for specified duration. 

**Suggested Solution:**
Expert team member wants to export ticket data and clicks on the ‘+’ icon from the messaging extension. There will be single message action menu item titled “Export”. On click, a task module will be invoked which will have option to export data for requests that have been created for the selected duration. The default values will be 
* Last 30 days
* Last 60 days
* Last 90 days
The task module will also have a button titled Export which on click will export ticket data to the SME users personal one drive

The first cell in the csv file will display the selected duration. Request data will be stored in from 3rd row onwards with following fields:
* Request Number
* Request title
* Request Description
* First Observed On date 