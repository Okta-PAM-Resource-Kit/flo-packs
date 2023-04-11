# Server Cleanup

# Note: This Okta Workflow is not supported by Okta, and no warranty is expressed or implied.  Please review and understand all scripts before using.  Use at your own risk.

This Okta Workflows Flo Pack is designed to help Okta ASA customers to proactively remove servers from their environment after they have been decommisioned.

Currently Okta ASA operates by removing inactive servers after a grace period of 96~ hours. For some customers this is too long.

There are some options when it comes to being proactive to clean up the ASA GUI with regards to inactive servers;

* AWS Server Discovery - This feature hooks into AWS and helps customers identify newly provisioned servers that are not yet protected by ASA and automatically removes decommisioned servers from ASA (https://help.okta.com/asa/en-us/Content/Topics/Adv_Server_Access/docs/aws-overview.htm)

* Automation Tools - Some organisations utilize the Okta ASA API to automatically remove inactive servers from a project when the server is decommisioned. The 'Remove a Server from a Project' is available here: (https://developer.okta.com/docs/reference/api/asa/projects/#remove-a-server-from-a-project) 

* Manual - This should be the last option considered as it is very manual and time consuming. However if a customer wanted to they could manually delete a server from the ASA GUI.

# Explanation of Server Cleanup Flo Pack

The Okta Workflows Flo Pack here is a middle ground option for customers that may not have full automation capability but don't want to wait 96 hours before servers are removed automatically.

The Server Cleanup Flo Pack works on a schedule defined by the customer and will query the Okta ASA /servers endpoint using an ASA Service Account (created by the customer). This ASA Service Account needs to be a member of all projects to be able to read information about all servers.

This will return server information to Okta Workflows which then get passed into a helper script which will go through all servers and check their 'last_seen' status. 

In Okta ASA, active servers will have their 'last_seen' status updated at least once every 24 hours. With this in mind, we can expect that a server with a 'last_seen status of greater than 24 hours is inactive.

Using this information, the helper script will look for servers that have a 'last_seen' status of greater than 24 hours and then using a Custom API action card remove the server in question.

# Setup

* Create ASA Service Account 
* Ensure Service Account is a member of all Projects
* In Okta Workflows ensure you have setup a connection to your ASA Environment
* Upload 'service-cleanup.folder' into Okta Workflows
* Open 'Inactive Servers Cleanup'
* Ensure that all Cards have appropriate connections
* Save and Enable the Flo
* Open the 'For Each' Flo
* Ensure that all Cards have appropraite connections
* NOTE: The default setting for the 'last_seen' value is set to 24. This Condition is within the 'If/Else' branch and can be altered if needed.
* Save and Enable the Flo

# Note: This Okta Workflows Flo Pack is not supported by Okta and needs to be fully tested before use to ensure it works as expected.
