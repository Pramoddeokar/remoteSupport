# Deployment Guide
To begin, you will need: 

* An Azure subscription where you can create the following kind of resources:
	* App service

	* App service plan

	* Bot channels registration

	* Azure storage account

	* Azure search

	* Application Insights    

* A team in Microsoft Teams with your group of experts. (You can add and remove team members later!)
* A copy of the Remote support app GitHub repo [Git URL](https://github.com/OfficeDev/microsoft-teams-apps-remotesupport)

### Step 1: Register Azure AD applications
Register two Azure AD applications in your tenant's directory: one for the bot, and another for the configuration app.

1. Log in to the Azure Portal for your subscription, and go to the "App registrations" blade [here](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps).

2. Click on "New registration", and create an Azure AD application.
	1. **Name**: The name of your Teams app - if you are following the template for a default deployment, we recommend "Remote support".
	2. **Supported account types**: Select "Accounts in any organizational directory"
	3. Leave the "Redirect URL" field blank.

[[/Images/multitenant_app_creation.png|Azure AD app registration page]]

3. Click on the "Register" button.

4. When the app is registered, you'll be taken to the app's "Overview" page. Copy the **Application (client) ID**; we will need it later. Verify that the "Supported account types" is set to **Multiple organizations**.

[[/Images/multitenant_app_overview.png|Azure AD app overview page]]

5. On the side rail in the Manage section, navigate to the "Certificates & secrets" section. In the Client secrets section, click on "+ New client secret". Add a description for the secret and select an expiry time. Click "Add".

[[/Images/multitenant_app_secret.png|Azure AD app overview page]]

6. Once the client secret is created, copy its **Value**; we will need it later.

At this point you have 3 unique values:

* Application (client) ID for the bot
* Client secret for the bot
* Directory (tenant) ID, which is the same for both apps

We recommend that you copy these values into a text file, using an application like Notepad. We will need these values later.

### Step 2: Deploy to your Azure subscription

1. Click on the "Deploy to Azure" button below.
 
   [![Deploy to Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FOfficeDev%2Fmicrosoft-teams-remotesupport-app%2Fmaster%2FDeployment%2Fazuredeploy.json)

2. When prompted, log in to your Azure subscription.

3. Azure will create a "Custom deployment" based on the ARM template and ask you to fill in the template parameters.

   [[/Images/CustomDeployment.png|CustomDeployment]]

4. Select a subscription and resource group.
* We recommend creating a new resource group.
* The resource group location MUST be in a datacenter that supports: Application Insights; and  Azure Search. For an up-to-date list, click [here](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=logic-apps,cognitive-services,search,monitor), and select a region where the following services are available:
* Application Insights
* Azure Search

5. Enter a "Base Resource Name", which the template uses to generate names for the other resources.
* The app service names `[Base Resource Name]`, `[Base Resource Name]-config`, must be available. For example, if you select `contosoremotesupport` as the base name, the names `contosoremotesupport`, and `contosoremotesupport-config` must be available (not taken); otherwise, the deployment will fail with a Conflict error.
* Remember the base resource name that you selected. We will need it later.

6. Fill in the various IDs in the template:

	1. **Bot Client ID**: The application (client) ID of the Microsoft Teams Bot app

	2. **Bot Client Secret**: The client secret of the Microsoft Teams Bot app

	3. **Tenant Id**: The tenant ID above

Make sure that the values are copied as-is, with no extra spaces. The template checks that GUIDs are exactly 36 characters.

7. If you wish to change the app name, description, and icon from the defaults, modify the corresponding template parameters.

8. Add the team link in team link input field. Get the link to the team with your experts from the Teams client. To do so, open Microsoft Teams, and navigate to the team. Click on the "..." next to the team name, then select "Get link to team".

9. Click on "Review+Create" to start the deployment.It will validate the parameters provided in the template .Once the validation is passed,click on create to start the deployment.

   [[/Images/Validationpassed.png|Deploy to Azure]]

10. Wait for the deployment to finish. You can check the progress of the deployment from the "Notifications" pane of the Azure Portal. It can take more than 10 minutes for the deployment to finish.

11. After the deployment is successful, open the deployment details blade and click on "Outputs" option visible in the navigation menu on the left and note down the mentioned values below. We will need them later in the deployment process.

* botId - This is the Microsoft Application ID for the Remote support bot.
* appDomain - This is the base domain for the Remote support Bot.
* configurationAppUrl - This is the URL for the configuration web application.
 
  [[/Images/output.png|Deploy to Azure]]


### Step 3: Create the Teams app packages

Create two Teams app packages: one for end-users to install personally, and one to be installed to the experts team.

1. Open the `Manifest\manifest_enduser.json` file in a text editor.

2. Change the placeholder fields in the manifest to values appropriate for your organization.

* `developer.name` ([What's this?](https://docs.microsoft.com/en-us/microsoftteams/platform/resources/schema/manifest-schema#developer))

* `developer.websiteUrl`

* `developer.privacyUrl`

* `developer.termsOfUseUrl`

3. Change the `<<botId>>` placeholder to your Azure AD application's ID from above. This is the same GUID that you entered in the template under "Bot Client ID".

4. In the "validDomains" section, replace the `<<appDomain>>` with your Bot App Service's domain. This will be `[BaseResourceName].azurewebsites.net`. For example if you chose "contosoremotesupport" as the base name, change the placeholder to `contosoremotesupport.azurewebsites.net`.

5. Save and Rename `manifest_enduser.json` file to a file named `manifest.json`.

6. Create a ZIP package with the `manifest.json`,`color.png`, and `outline.png`. The two image files are the icons for your app in Teams.
* Name this package `remotesupport-enduser.zip`, so you know that this is the app for end-users.
* Make sure that the 3 files are the _top level_ of the ZIP package, with no nested folders.

[[/Images/file-explorer.png|Manifest]]

7. Delete the `manifest.json` file.

Repeat the steps above but with the file `Manifest\manifest_sme.json`. Name the resulting package `remotesupport-experts.zip`, so you know that this is the app for experts.

### Step 4: Run the apps in Microsoft Teams

1. If your tenant has sideloading apps enabled, you can install your app by following the instructions
[here](https://docs.microsoft.com/en-us/microsoftteams/platform/concepts/apps/apps-upload#load-your-package-into-teams)

2. You can also upload it to your tenant's app catalog, so that it can be available for everyone in your tenant to install. See [here](https://docs.microsoft.com/en-us/microsoftteams/tenant-apps-catalog-teams)

3. Install the experts app (the `remotesupport-experts.zip` package) to your team of subject-matter experts.

* We recommend using [app permission policies](https://docs.microsoft.com/en-us/microsoftteams/teams-app-permission-policies) to restrict access to this app to the members of the experts team.

4. Install the end-user app (the `remotesupport-enduser.zip` package) to your users.

### Troubleshooting

Please see our [Troubleshooting](https://github.com/OfficeDev/microsoft-teams-apps-remotesupport/wiki/Troubleshooting) page.