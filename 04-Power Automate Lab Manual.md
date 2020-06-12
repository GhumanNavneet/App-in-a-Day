Power Platform
App in a Day
Module 4: Power Automate
Hands-on Lab Step-by-Step
May 2020
Contents
Power Automate....................................................................................................................................................................1
Lab Prerequisites ............................................................................................................................................................................................................... 1
Exercise 1: Create Approval Request Flow................................................................................................................................................................ 2
Exercise 2: Conditional Logic.......................................................................................................................................................................................10
Exercise 3: Test the Flow ...............................................................................................................................................................................................16
Exercise 4: Update the Flow.........................................................................................................................................................................................20
Lab survey ............................................................................................................................................................................26
References ............................................................................................................................................................................26
Copyright .............................................................................................................................................................................27
Power Platform App in a Day Module 4: Power Automate
1 | P a g e ©2020 Microsoft Corporation
Power Automate
Lab Prerequisites
This is the fourth lab in a five-part series covering Power Apps, Common Data Service, and Power Automate. The
assumption is that you have successfully completed the first three modules, or at least the initial part of setting up an
environment as described in the overview – “00-AppInADay Lab Overview.pdf”.
If you have not completed the previous modules, you can use the partially completed version of the lab package in the
“\Completed\Module3” folder. Follow the instructions in the document “Importing Module 3 Completed” before
proceeding with this module, which will provision the app and the Common Data Service entity into your environment.
Integrating a Power Apps App with Power Automate
In this lab, you will create a flow that uses the Modern Approvals service to automate the approval workflow – it will send
an email to the selected approver and take an action based on their response.
You should already have an app with these two screens:
Power Platform App in a Day Module 4: Power Automate
©2020 Microsoft Corporation 2 | P a g e
Exercise 1: Create Approval Request Flow
The flow will trigger when a new item is added to the Device Order entity table in the Common Data Service.
• It will use the Approvals Service to send an approval request.
• The approver will receive an email with options to Approve or Rejects and add comments.
• Once the approver responds, the record in the Device Order table will be updated with the appropriate approval
status and comments.
• An email will be sent to the requester informing them whether the device was approved or rejected.
There are two ways to create a flow – from blank or from a template. In this lab, we will create the approval flow
starting with a blank flow.
Task 1: Login on Power Apps website and create a flow
1. Navigate to Make Power Apps and make sure you are in the correct environment.
2. Select Flows.
3. Click New and select Automated – from Blank.
Power Platform App in a Day Module 4: Power Automate
3 | P a g e ©2020 Microsoft Corporation
Task 2: Configure the trigger
The first thing you will need to configure is the trigger, i.e. when should this flow run. A flow can be triggered:
a. manually from a Power Apps app,
b. manually from a flow button,
c. on a fixed schedule, or
d. when an event occurs, such as a new item being added to a table, a new email arriving in a user’s inbox, a
new tweet being posted that meets certain conditions, etc.
In this scenario, we will configure the flow to trigger when a new item is added to the Device Order entity table in the
Common Data Service
1. Enter a name for your flow, such as – “New device approval request”
2. In the Choose your flow’s trigger box, enter Common Data Service and select When a record is created -
Common Data Service.
3. Click Create
4. Click the Environment drop-down and select Current.
5. Click the Entity Name drop-down and select Device Orders. You can type “device orders” to search for it.
Power Platform App in a Day Module 4: Power Automate
©2020 Microsoft Corporation 4 | P a g e
6. Click the Scope drop-down and select Organization. Scope allows you to limit when your flow will run, for
example you could choose User and it would only run for orders you create. In this case you are choosing
organization because you want this flow to run for records created by anyone in your entire organization.
Task 3: Add action to send an approval request
1. Click + New step.
2. Search for Approvals and select Start and wait for an approval. 
Power Platform App in a Day Module 4: Power Automate
5 | P a g e ©2020 Microsoft Corporation
This will use the modern approval service. For more information see the blog post at Flow Modern Approvals.
3. In the Approval type dropdown select Approve/Reject - First to Respond.
4. For the Title, we will add some text and one variable. This variable will contain the Device Name of the device
order request. Enter New device request for in the Title text box.
5. Select Device Name for the Dynamic content.
Note: if the Dynamic content box is not visible, click the Add dynamic content button -
6. Select the Assigned to field, select click Approver. Click on the Add dynamic content button to show/hide the
dynamic content pane.
Power Platform App in a Day Module 4: Power Automate
©2020 Microsoft Corporation 6 | P a g e
You might get a warning message about this field being optional. Ignore it and ignore similar warnings in future.
Note: Recall from the earlier lab that this will be the approver’s email address.
7. Click Show Advanced Options.
8. Select the Requestor field and select Requested By
9. In the Details field, type A new device has been requested and hit <Enter>.
Power Platform App in a Day Module 4: Power Automate
7 | P a g e ©2020 Microsoft Corporation
10. Select Device Name from the Dynamic content pane.
11. Type , $ and select Price. You may need to click the "See More" option under the dynamic content search bar in
order to see the Price option.
12. Hit Enter and type Department Contribution: $
13. Select Department Contribution.
14. Hit Enter, type Comments: and select Comments.
Power Platform App in a Day Module 4: Power Automate
©2020 Microsoft Corporation 8 | P a g e
15. Your Flow will now look like the image below.
Power Platform App in a Day Module 4: Power Automate
9 | P a g e ©2020 Microsoft Corporation
16. Save your flow
Note: When creating your own approval flows, you may additionally include a clickable link that will be displayed in the
approval email. In this scenario, for example, you could include a link to view device details in an online catalogue. You
would include the Item link and Item link description.
Note: You could also set the Item link to deep link into a Power Apps app to view more details about the request. In this
scenario, you might pass an OrderID or a DeviceID as a URL parameter. Power Apps accepts URL parameters, see Flow URL
Patameters for more details.
Power Platform App in a Day Module 4: Power Automate
©2020 Microsoft Corporation 10 | P a g e
Exercise 2: Conditional Logic
In flow, you can add conditions to take different actions depending on a certain result, in this case, whether the request
was approved or rejected.
Task 1: Add conditional logic to flow
1. Click + New step.
2. Search for Condition and select it.
3. Click in the left edit box that says, “Choose a value” and select Outcome from the dynamic content pane. You may
need to press the “+” icon below the edit box to hide the dynamic content pane.
Power Platform App in a Day Module 4: Power Automate
11 | P a g e ©2020 Microsoft Corporation
4. Select is equal to for condition and type Approve for Value.
Task 2: Add conditional logic to flow
We will now configure what actions to perform if the response is approved or not – YES branch vs. NO branch.
We will add two actions:
a. Update the record in the Device Order table
b. Send an email to the employee who requested the device
1. In the left If yes box, click Add an action
2. Search for Common Data Service and select Common Data Service – Update a record
3. Select Current for Environment.
Power Platform App in a Day Module 4: Power Automate
©2020 Microsoft Corporation 12 | P a g e
4. Select Device Orders for Entity Name.
5. Select Device Order for Record identifier.
This is the unique lookup ID for the record that was created.
6. Click Show advanced options.
7. Select Approve from the Approval Status Value drop-down.
8. Select the Approved Date field and select the Expression tab.
9. Type utcNow() and click OK.
Power Platform App in a Day Module 4: Power Automate
13 | P a g e ©2020 Microsoft Corporation
10. In the Comments field, we want to preserve the earlier comments and append on the comments from the
approver. To do so, select the Comments field and select Comments.
11. Save the flow.
Task 3: Add another action
You will now add the send email action to the If Yes branch.
1. From within the yes branch, Click Add an Action.
2. Search for send email and select Send an email (V2) – Office 365 Outlook.
Power Platform App in a Day Module 4: Power Automate
©2020 Microsoft Corporation 14 | P a g e
3. Click Sign in.
4. Click Accept.
5. Click on the To field and click Switch to Advanced Mode.
6. Select Requested By for To. Select from under the When a record is created section.
Is 
Power Platform App in a Day Module 4: Power Automate
15 | P a g e ©2020 Microsoft Corporation
7. Type Your device order has been approved! for Subject.
8. Click on the Code View button.
9. Set the Body value as shown below. Select Device Name and Estimated Ship Date from underneath the When
a record is created header.
Note: If you do not have an Office 365 mailbox setup, you can use one of the other connectors to send the email, such as
Outlook.com, Gmail or SendGrid.
10. Click Save.
Power Platform App in a Day Module 4: Power Automate
©2020 Microsoft Corporation 16 | P a g e
Exercise 3: Test the Flow
To test the flow, you will:
a. Run the Device Ordering app and submit an approval request
b. Verify the request was sent to the approver
c. Approve the request
d. Verify that the Common Data Service record was updated, and an email was sent back to the requestor
Task 1: Test the Flow
Note: When a new device record is added to the Device Order entity in CDS, it may take up to ten minutes for the flow to
trigger. To ensure the flow runs immediately, select the Test option in the top right and select the “I’ll perform the
trigger action” option. Then go ahead and submit a device request. The flow should run immediately.
1. Select I’ll Perform the Trigger Action and click Save & Test.
2. To submit a device request, go to Make Power Apps
3. Select Apps and start the Device Ordering App.
Power Platform App in a Day Module 4: Power Automate
17 | P a g e ©2020 Microsoft Corporation
4. Select a few devices and click Compare.
5. Select one of the devices, provide email for Approver.
6. Provide a comment and click Submit device request.
Power Platform App in a Day Module 4: Power Automate
©2020 Microsoft Corporation 18 | P a g e
7. Click OK.
8. The flow will run and send email to the manager email you provided. The request for approval email will look like
the image below; it will include Device information, Price, Department Contribution (the calculated field), and
the Requester Comment.
REMINDER: If the flow does not run immediately, please wait, it may take up to ten minutes for the flow to be
triggered. To ensure the flow runs immediately, see note above - select the Test option in the top right and select
the “I’ll perform the trigger action” option. Then go ahead and submit a device request. The flow should run
immediately. The email, however, may take a few minutes to appear regardless of when the flow starts.
9. Click Approve.
10. Add a comment and click Submit.
Power Platform App in a Day Module 4: Power Automate
19 | P a g e ©2020 Microsoft Corporation
11. The flow will continue to run; it will update the record and send an email to the requestor. The email sent to the
requester will look like the image below.
12. Check the flow, you will notice that the flow is now marked as Succeeded in the run history.
Power Platform App in a Day Module 4: Power Automate
©2020 Microsoft Corporation 20 | P a g e
Exercise 4: Update the Flow
In this exercise, you will add two actions to the “if no” branch.
Task 1: Add actions
1. If you don’t already have the flow open, open it in edit mode.
2. In the If no branch, click Add an action.
3. Search for Common Data Service and select Common Data Service – Update a record.
Power Platform App in a Day Module 4: Power Automate
21 | P a g e ©2020 Microsoft Corporation
4. Select Current for environment, Device Orders for Entity Name, select Device Order for Record Identifier, and
click Show advanced options
5. Select Reject for Approval Status Value.
6. Click Add an action.
7. Search for send email and select Send an email (v2) - Office 365 Outlook.
Power Platform App in a Day Module 4: Power Automate
©2020 Microsoft Corporation 22 | P a g e
8. Provide the information shown on the image below. This will send an email to the requestor informing them that
their device request was not approved. Select Requested By and Device Name from under the When a record is
created header.
9. Save the flow.
Task 2: Test the updated Flow
1. Click Test in the top right of the flow editor and start the Flow.
2. Run the Device Ordering app -> Select a device and submit an approval request.
3. You should receive an email with options to Approve or Reject the request. Select Reject this time and enter some
comments, such as “Not eligible for new device.” Click Submit.
4. Confirm that the requestor receives an email informing them that their device approval request was rejected.
Power Platform App in a Day Module 4: Power Automate
23 | P a g e ©2020 Microsoft Corporation
5. Navigate to Make Power Apps select Apps and start the Device Procurement application.
6. Device Orders will now have the Approval Status.
Task 3: Visit the approval center
1. Use the Device Ordering app to submit a few more approval requests.
2. Navigate to Power Automate and make sure you are in the correct environment. Login with your lab credentials if
prompted.
3. Expand Action items and select Approvals.
Power Platform App in a Day Module 4: Power Automate
©2020 Microsoft Corporation 24 | P a g e
4. Notice that all pending approval requests are visible.
5. Go ahead and approve or reject a request from this screen. The details are displayed in the right pane where you
can enter comments and Confirm.
6. The request will no longer be visible as it has been processed.
Power Platform App in a Day Module 4: Power Automate
25 | P a g e ©2020 Microsoft Corporation
Note: All approval requests sent to the current logged on user will be visible in the Approvals Center. This includes approvals
sent from any app or flow.
7. You can also use the Approvals Center to view all requests that you have sent and are Awaiting response from
the approver. Select the Sent requests tab at the top to view all requests that you have sent.
8. Open the Power Automate mobile app on your mobile device.
9. Login and switch to the environment where the flow is deployed.
10. Select Approvals in the top right and view all pending approvals.
11. You can quickly approve or reject these pending requests from this screen.
12. If you have push notifications turned on and are signed into the flow mobile app – when you receive a new
Approval request it will trigger a push notification on your phone. You can give this a shot.
Power Platform App in a Day Module 4: Power Automate
©2020 Microsoft Corporation 26 | P a g e
Congratulations! You have successfully completed this lab. You have created your Power Apps app and flow and
connected them to a Common Data Service entity. Now you are ready to build your own apps and workflows.
Lab survey
We would appreciate your feedback on the Business Application Platform technologies and on this hands-on-lab, such as
the quality of documentation and the usefulness of the learning experience.
Please use the survey at App in a day survey to share your feedback.
You may provide feedback for each module as you complete it or at the end once you’ve completed all the modules.
Thank you!
References
App in a Day introduces some of the key functionalities available in Power Apps, Power Automate, Power BI and the
Common Data Service. For an up to date list of learning references, see Power Apps Resources and Power Automate
Resources and Power BI. 
Power Platform App in a Day Module 4: Power Automate
27 | P a g e ©2020 Microsoft Corporation
Copyright
© 2020 Microsoft Corporation. All rights reserved.
By using this demo/lab, you agree to the following terms:
The technology/functionality described in this demo/lab is provided by Microsoft Corporation for purposes of obtaining
your feedback and to provide you with a learning experience. You may only use the demo/lab to evaluate such technology
features and functionality and provide feedback to Microsoft. You may not use it for any other purpose. You may not
modify, copy, distribute, transmit, display, perform, reproduce, publish, license, create derivative works from, transfer, or
sell this demo/lab or any portion thereof.
COPYING OR REPRODUCTION OF THE DEMO/LAB (OR ANY PORTION OF IT) TO ANY OTHER SERVER OR
LOCATION FOR FURTHER REPRODUCTION OR REDISTRIBUTION IS EXPRESSLY PROHIBITED.
THIS DEMO/LAB PROVIDES CERTAIN SOFTWARE TECHNOLOGY/PRODUCT FEATURES AND FUNCTIONALITY,
INCLUDING POTENTIAL NEW FEATURES AND CONCEPTS, IN A SIMULATED ENVIRONMENT WITHOUT
COMPLEX SET-UP OR INSTALLATION FOR THE PURPOSE DESCRIBED ABOVE. THE TECHNOLOGY/CONCEPTS
REPRESENTED IN THIS DEMO/LAB MAY NOT REPRESENT FULL FEATURE FUNCTIONALITY AND MAY NOT
WORK THE WAY A FINAL VERSION MAY WORK. WE ALSO MAY NOT RELEASE A FINAL VERSION OF SUCH
FEATURES OR CONCEPTS. YOUR EXPERIENCE WITH USING SUCH FEATURES AND FUNCTIONALITY IN A
PHYSICAL ENVIRONMENT MAY ALSO BE DIFFERENT.
FEEDBACK. If you give feedback about the technology features, functionality and/or concepts described in this demo/lab
to Microsoft, you give to Microsoft, without charge, the right to use, share and commercialize your feedback in any way
and for any purpose. You also give to third parties, without charge, any patent rights needed for their products,
technologies and services to use or interface with any specific parts of a Microsoft software or service that includes the
feedback. You will not give feedback that is subject to a license that requires Microsoft to license its software or
documentation to third parties because we include your feedback in them. These rights survive this agreement.
MICROSOFT CORPORATION HEREBY DISCLAIMS ALL WARRANTIES AND CONDITIONS WITH REGARD TO THE
DEMO/LAB, INCLUDING ALL WARRANTIES AND CONDITIONS OF MERCHANTABILITY, WHETHER EXPRESS,
IMPLIED OR STATUTORY, FITNESS FOR A PARTICULAR PURPOSE, TITLE AND NON-INFRINGEMENT.
MICROSOFT DOES NOT MAKE ANY ASSURANCES OR REPRESENTATIONS WITH REGARD TO THE ACCURACY
OF THE RESULTS, OUTPUT THAT DERIVES FROM USE OF DEMO/ LAB, OR SUITABILITY OF THE INFORMATION
CONTAINED IN THE DEMO/LAB FOR ANY PURPOSE.
DISCLAIMER
This demo/lab contains only a portion of new features and enhancements in Microsoft Power Apps. Some of the features
might change in future releases of the product. In this demo/lab, you will learn about some, but not all, new features.
