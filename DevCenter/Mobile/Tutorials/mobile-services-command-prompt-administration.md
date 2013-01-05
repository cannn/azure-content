<properties linkid="develop-mobile-tutorials-command-prompt-administration" urlDisplayName="Command prompt administration" pageTitle="Automate mobile services with command-line tools" metaKeywords="Windows Azure Mobile Services, command prompt, command line tool, mobile services" metaDescription="Learn how to use the Windows Azure command-line tool to automate the creation of management of Windows Azure Mobile Services." metaCanonical="" disqusComments="1" umbracoNaviHide="1" />

<div title="This is rendered content from macro" class="umbMacroHolder" onresizestart="return false;" umbpageid="15161" umbversionid="f1a70b05-645d-4fcd-bb15-74674509c46a" ismacro="true" umb_chunkpath="devcenter/Menu" umb_modaltrigger="" umb_chunkurl="" umb_hide="0" umb_chunkname="MobileArticleLeft" umb_modalpopup="0" umb_macroalias="AzureChunkDisplayer"><!-- startUmbMacro --><span><strong>Azure Chunk Displayer</strong><br />No macro content available for WYSIWYG editing</span><!-- endUmbMacro --></div>

# Automate mobile services with command-line tools 

This topic shows you how to use use the Windows Azure command-line tools to automate the creation and management of Windows Azure Mobile Services. This topic shows you how to install and get started using the command-line tools and use them to perform the following Mobile Services tasks:

-	[Create a new mobile service] 
-	[Create a new table]
-   [Register a script to a table operation][Register a new table script]
- 	[Delete an existing table]
-   [Delete an existing mobile service]
 
When combined into a single script or batch file, these individual commands automate the creation, verfication, and deletion process of a mobile service. 

To use the Windows Azure command-line tools to manage Mobile Services, you need a Windows Azure account that has the Windows Azure Mobile Services feature enabled.

+ If you don't have an account, you can create a free trial account in just a couple of minutes. For details, see <a href="http://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A0E0E5C02" target="_blank">Windows Azure Free Trial</a>.

+ If you have an existing account but need to enable the Windows Azure Mobile Services preview, see <a href="../create-a-windows-azure-account/#enable" target="_blank">Enable Windows Azure preview features</a>.

This topic covers a selection of common administration tasks supported by the Windows Azure command-line tools. For more information, see [Windows Azure command-line tools documentation][reference-docs].

<!--+  You must download and install the Windows Azure command-line tools to your local machine. To do this, follow the instructions in the first section of this topic. 

+ (Optional) To be able to execute HTTP requests directly from the command-line, you must use cURL or an equivalent tool. cURL runs on a variety of platforms. Locate and install cURL for your specific platform from the <a href=http://go.microsoft.com/fwlink/p/?LinkId=275676 target="_blank">cURL download  page</a>.-->

<h2><a name="install"></a><span class="short-header">Install the tools</span>Install the Windows Azure Command-Line Tools</h2>

The following list contains information for installing the command-line tools, depending on your operating system:

* **Windows**: Download the [Windows Azure Command-Line Tools Installer][windows-installer]. Open the downloaded .msi file and complete the installation steps as you are prompted.

* **Mac**: Download the [Windows Azure SDK Installer][mac-installer]. Open the downloaded .pkg file and complete the installation steps as you are prompted.

* **Linux**: Install the latest version of [Node.js][nodejs-org] (see [Install Node.js via Package Manager][install-node-linux]), then run the following command:

		npm install azure-cli -g

	**Note**: You may need to run this command with elevated privileges:

		sudo npm install azure-cli -g

To test the installation, type `azure` at the command prompt. When the installation is successful, you will see a list of all the available `azure` commands.
<h2><a name="import-account"></a><span class="short-header">Import settings</span>How to download and import publish settings</h2>

To get started, you need to first download and import your publish settings. This will allow you to use the tools to create and manage Azure Services. To download your publish settings, use the `account download` command:

	azure account download

This will open your default browser and prompt you to sign in to the Management Portal. After signing in, your `.publishsettings` file will be downloaded. Make note of where this file is saved.

Next, import the `.publishsettings` file by running the following command, replacing `<path-to-settings-file>` with the path to your `.publishsettings` file:

	azure account import <path-to-settings-file>

You can remove all of the information stored by the <code>import</code> command by using the <code>account clear</code> command:

	azure account clear

To see a list of options for `account` commands, use the `-help` option:

	azure account -help

After importing your publish settings, you should delete the `.publishsettings` file for security reasons.

<div class="dev-callout"><strong>Note</strong> 
   <p>When you import publish settings, credentials for accessing your Windows Azure subscription are stored inside your <code>user</code> folder. Your <code>user</code> folder is protected by your operating system. However, it is recommended that you take additional steps to encrypt your <code>user</code> folder. You can do so in the following ways:</p>

   	<ul>
		<li>On Windows, modify the folder properties or use BitLocker.</li>
		<li>On Mac, turn on FileVault for the folder.</li>
		<li>On Ubuntu, use the Encrypted Home directory feature. Other Linux distributions offer equivalent features.</li>
	</ul>
</div>

You are now ready to begin creating and managing Windows Azure Mobile Services from the command line or in batch files.  

<h2><a name="create-service"></a><span class="short-header">Create service</span>How to create a new mobile service</h2>

You can use the command-line tools to create a new mobile service instance. While creating the mobile service, you also create a new database instance in a new SQL Database server. 

The following command creates a new mobile service instance in your subscription, where `<service-name>` is the name of the new mobile service, `<server-admin>` is the login name of the new server, and `<server-password>` is the password for the new login:

		azure mobile create <service-name> <server-admin> <server-password>

<div class="dev-callout"><strong>Security Note</strong> 
   <p><code>&lt;server-admin&gt;</code> and <code>&lt;server-password&gt;</code> are important security credentials. Because of this, we recommend that you take additional steps to encrypt any file or folder where these credentials are persisted. When you do not supply values for <code>&lt;server-admin&gt;</code> and <code>&lt;server-password&gt;</code> in the <code>mobile create</code> command, you are prompted to supply the values during execution.</p>
</div>

The `mobile create` command fails when the specified mobile service already exists. In your automation scripts, you may want to attempt to delete a mobile service before attempting to recreate it.

<h2><a name="delete-service"></a><span class="short-header">Delete service</span>How to delete an existing mobile service</h2>

You can use the command-line tools to delete an existing mobile service, along with the related SQL Database and server. The following command deletes the mobile service, where `<service-name>` is the name of the mobile service to delete:

		azure mobile delete <service-name> -a -q

By including `-a` and `-q` parameters, this command also deletes the SQL Database and server used by the mobile service without displaying a prompt.

<div class="dev-callout"><strong>Note</strong> 
   <p>If you do not specify the <code>-q</code> parameter along with <code>-a</code> or <code>-d</code>, execution is paused and you are prompted to select delete options for your SQL Database. Only use the <code>-a</code> parameter when no other service using the database or server; otherwise use the <code>-d</code> parameter to only delete data that belongs to the mobile service being deleted.</p>
</div>

<h2><a name="create-table"></a><span class="short-header">Create table</span>How to create a new table in the mobile service</h2>

The following command creates a new table in the specified mobile service, where `<service-name>` is the name of the mobile service and `<table-name>` is the name of the table to create:

		mobile table create <service-name> <table-name>

This creates a new table with the default permissions, `application`, for the table operations: `insert`, `read`, `update`, and `delete`. The following command creates a new table with `read` permission set to public and `delete` permission set to only adminstrators:

		mobile table create <service-name> <table-name> -p read=public,delete=admin

The following table shows the script permission value compared to the permission value in the [Windows Azure Management Portal].

<table border="1" width="100%"><tr><th>Script value</th><th>Management Portal value</th></tr>
<tr><td><code>public</code></td><td>Everyone</td></tr>
<tr><td><code>application</code> (default)</td><td>Anybody with the application key</td></tr>
<tr><td><code>user</code></td><td>Only authenticated users</td></tr>
<tr><td><code>admin	</code></td><td>Only scripts and admins</td></tr></table>

The `mobile table create` command fails when the specified table already exists. In your automation scripts, you may want to attempt to delete a table before attempting to recreate it.

<h2><a name="delete-table"></a><span class="short-header">Delete table</span>How to delete an existing table from the mobile service</h2>

The following command deletes a table from the mobile service, where `<service-name>` is the name of the mobile service and `<table-name>` is the name of the table to delete:

		azure mobile table delete <service-name> <table-name> -q

In automation scripts, use the `-q` parameter to delete the table without displaying a confirmation prompt that blocks execution.

<h2><a name="register-script"></a><span class="short-header">Register a script</span>How to register a script to a table operation</h2>

Mobile Services enables you to register a JavaScript function to a given table operation, where the script is run when the table operation occurs. The following command uploads and registers a function to an operation on a table, where `<service-name>` is the name of the mobile service, `<table-name>` is the name of the table, and `<operation>` is the table operation, which can be `read`, `insert`, `update`, or `delete`:

		azure mobile script upload <service-name> table/<table-name>.<operation>.js

Note that this operation uploads a JavaScript (.js) file from the local computer. The name of the file must be composed  from the table and operation names, and it must be located in the `table` subfolder relative to the location where the command is executed. For example, the following operation uploads and registers a new `insert` script that belongs to the `TodoItems` table:

		  azure mobile script upload todolist table/todoitems.insert.js

The function declaration in the script file must also match the registered table operation. This means that from the above example, the uploaded script contains a function with the following signature:

		function insert(item, user, request) {
		    ...
		} 

For more information about registering scripts, see [Mobile Services server script reference].

<!--<h2><a name="test-service"></a><span class="short-header">Test the service</span>Test the new mobile service</h2>

When you are automating the creation of your mobile service, you can optionally use cURL or another command-line request generator to 

## <a name="nextsteps"> </a>Next Steps
Next steps here....
-->
<!-- Anchors. -->
[Download and install the command-line tools]: #install
[Download and import publish settings]: #import
[Create a new mobile service]: #create-service
[Get the master key]: #get-master-key
[Create a new table]: #create-table
[Register a new table script]: #register-script
[Delete an existing table]: #delete-table
[Delete an existing mobile service]: #delete-service
[Test the mobile service]: #test-service
[Next steps]: #next-steps

<!-- Images. -->
[1]: ../Media/mobile-portal-data-tables-channel.png
[2]: ../Media/mobile-insert-script-channel-clear.png
[3]: ../Media/mobile-services-selection.png
[4]: ../Media/mobile-schedule-new-job.png
[5]: ../Media/mobile-create-job-dialog.png
[6]: ../Media/mobile-schedule-job-script-new.png
[7]: ../Media/mobile-schedule-job-script.png
[8]: ../Media/mobile-verify-channel-duplicates.png
[9]: ../Media/mobile-schedule-job-logs.png
[10]: ../Media/mobile-schedule-job-enabled.png

<!-- URLs. -->
[Mobile Services server script reference]: http://go.microsoft.com/fwlink/p?LinkId=262293
[WindowsAzure.com]: http://www.windowsazure.com/
[Windows Azure Management Portal]: https://manage.windowsazure.com/
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://www.microsoft.com/web/downloads/platform.aspx
[mac-installer]: http://go.microsoft.com/fwlink/p?LinkId=252249
[windows-installer]: http://go.microsoft.com/fwlink/p?LinkID=275464
[reference-docs]: http://go.microsoft.com/fwlink/p?LinkId=252246
[http://www.windowsazure.com]: http://www.windowsazure.com