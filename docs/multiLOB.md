# Multiple Lines of Business Using the Same Flow

## Story
> You do not always need to create a new flow for each line of business in the contact center, instead you can often reuse your flow logic and account for the differences including different queues and wait treatments.  In this lab you will be adding a new line of business to the CL<w class="POD"></w>_core</copy> flow.


## Build
### Open flow <copy>CL<w class="POD"></w>_core</copy>
> Toggle the Edit switch on
>
---
### Replace the welcome Play Message node with a Menu node
> Click on the welcome Play Message and delete it.
>
> Add a new Menu node
> >
> > Activity Label: <copy>welcomeMenu</copy>
> >
> > Enable Text-To-Speech
> 
> > Select the Connector: Cisco Cloud Text-to-Speech
>
> > Click the Add Text-to-Speech Message button
>
> > Delete the Selection for Audio File
>
> > Text-to-Speech Message: <copy>Thank you for calling Acme Corp. where we have been helping coyotes reach space since 1949.</copy>
> 
> > Click the Add Text-to-Speech Message button
>
> > Text-to-Speech Message: <copy>For Sales, press 1.  For Service, press 2.</copy>
>
> > Select Make Prompt Interruptible
>
> > Digit Number: 1 Link Description: <copy>Sales</copy>
>
> > Digit Number: 2 Link Description: <copy>Service</copy>
>
> > Connect the No-Input Timeout node edge to the input node edge of this node
>
> > Connect the Unmatched Entry node edge to the input node edge of this node
>
> > Connect the Sales node edge to the Queue Contact node
> 
---

### Move the LunchCheck node after the Queue Contact node
> Delete the connection between the Queue Contact node and the Play Music node
>
> Delete the connection between the LunchCheck False node edge and the Queue Contact node
> 
> Connect the Queue Contact output node edge to the LunchCheck input node edge (you may want to move the nodes around to make things cleaner)
>
> Connect the LunchCheck False node edge to the Play Music node

---

### Add a Queue Contact node
> Activity Label: <copy>serviceQueue</copy>
> 
> Select Static Queue
>
> Queue: <copy><w class="Queue2">yourQueueID</w></copy>
>
> Connect the Service node edge from WelcomeMenu to the input node edge of this node
>
> Connect the output node edge of this node to the play music node
---



??? Note "If you have already completed Offering a Callback option to calls actively waiting in the queue"
    ### Update the Subflow Node with a new variable mapping
    > Open the Subflow node
    > 
    > Scroll down to the Subflow Input Variables
    >
    > Click Add New
    >
    > Current Flow Variable: <copy>welcomeMenu.OptionEntered</copy>
    >
    > Subflow Input Variable: <copy>LOB</copy>
    >
    ---
    
    ### Edit the first case in the CBchoice node
    > Open the CBchoice node
    >
    > Change the text in the first Link Description from {{cbChoice == false and BusinessHours.WorkingHoursShift_Name == "Lunch"}} to <copy>{{cbChoice == false and BusinessHours.WorkingHoursShift_Name == "Lunch" and welcomeMenu.OptionEntered == "1"}}</copy>

    ### <details><summary>Check your flow</summary>![](./assets/multiWithCBpart1.png)</details>

---



### Create a JSON flow variable
>  Name: <copy>LOBmessages</copy>
>
> Type: JSON
>
> Default Value: <copy>{}</copy>
---

### Add a new Function node
> Function Name: LOBwaitMessages
>
> Function Version Label: Live
>
> Function Input Variables:
>
> > Current Flow Variable: <copy>welcomeMenu.OptionEntered</copy>  Function input Variable: <copy>LOB</copy>
>
> Output Settings:
>
> > Output Variable: <copy>LOBmessages</copy>
>
> > Path Expression: <copy>$.messageList</copy>
>
> Delete the connection between the serviceQueue node and the Play Music node
>
> Connect the serviceQueue node to this Function node
>
> Delete the connection between the False node edge of LunchCheck and the Play Music node
>
> Connect the False node edge of LunchCheck to this Function node
>
> Connect the output node edge of this Function node to the Play Music node
>
---
??? Note "If you have already completed Offering a Callback option to calls actively waiting in the queue"
    ### Add a Play message node
    > Activity Label: <copy>LOBmessages1</copy>
    >
    > Enable Text-To-Speech
    >
    > Select the Connector: Cisco Cloud Text-to-Speech
    >
    > Click the Add Text-to-Speech Message button
    >
    > Delete the Selection for Audio File
    >
    > Text-to-Speech Message: <copy>{{LOBmessages[(counter % (LOBmessages| length))] }}</copy>
    > 
    > Delete the connection between the Set Variable node for the counter and the Condition node
    >
    > Connect the output node edge of Set Variable node to this Play Message node
    >
    > Connect the output node edge of this Play Message node to the Condition node
    ---

    ### Delete the old Play Message nodes
    > Delete both play message nodes which were previously used for the alternating messages
    >
    > Connect the True node edge of the Condition node to the Subflow node
    >
    > Connect the False node edge of the Condition node to the Play Music node
    ---

    ### Edit the Callback node settings
    > Open the Callback node
    >
    > Find the Register callback to different location switch and turn it off
    >
    ---

    ### <details><summary>Check your flow</summary>![](./assets/multiWithCBpart2.png)</details>

??? Note "If you have NOT completed Offering a Callback option to calls actively waiting in the queue"
    ### Add a Play message node
    > Activity Label: <copy>LOBmessages1</copy>
    >
    > Enable Text-To-Speech
    >
    > Select the Connector: Cisco Cloud Text-to-Speech
    >
    > Click the Add Text-to-Speech Message button
    >
    > Delete the Selection for Audio File
    >
    > Text-to-Speech Message: <copy>{{LOBmessages[(counter % (LOBmessages| length))] }}</copy>
    > 
    > Delete the Condition node
    >
    > Delete both play message nodes which were previously used for the alternating messages
    >
    > Connect the output node edge of Set Variable node to this Play Message node
    >
    > Connect the output node edge of this Play Message node to the Play Music node
    ---


    ### <details><summary>Check your flow</summary>![](./assets/multiNoCBpart2.png)</details>

---


### Publish your flow
> Turn on Validation at the bottom right corner of the flow builder
>
> If there are no Flow Errors, Click Publish
>
> Add a publish note
>
> Add Version Label(s): Test 
>
> Click Publish Flow

---


### Map your flow to your inbound channel
> Navigate to Control Hub > Contact Center > Channels
>
> Locate your Inbound Channel (you can use the search): <copy><w class="EP"></w></copy>
>
> Select the Routing Flow: <copy>CL<w class="POD"></w>_core</copy>
>
> Select the Version Label: Test
>
> Click Save in the lower right corner of the screen

---

### If you are still in the lunch portion of the day, adjust the lunch and Open PM Shifts to allow Open PM to be active

---

## Testing
1. Launch the [Agent Desktop](https://desktop.wxcc-us1.cisco.com/) and log in using the Desktop option.
      1. Make sure you are logged into <w class="Team2"><w>
      2. On your Agent Desktop, make sure your status is not set to available
2. Using Webex, place a call to your Inbound Channel number <copy><w class="DN"></w></copy>
      1. Choose option 1 for Sales
      2. If you have previously completed Offering a Callback option to calls actively waiting in the queue, do not elect for a callback
         1. You should hear alternating music and Sales messages 
            1. How many messages were there?
         2. End the call
3.  Using Webex, place a call to your Inbound Channel number <copy><w class="DN"></w></copy>
      1. Choose option 2 for Service
      2. If you have previously completed Offering a Callback option to calls actively waiting in the queue, do not elect for a callback
         1. You should hear alternating music and Service messages 
            1. How many messages were there?
         2. Set your Status to Available 
         3. After the call is connected end the call, set your status as NOT Available, wrap-up the call, and log back into <w class="Team"><w>


---

# Once you have completed the testing, go pick another adventure from the [Adventure Section](adventureList.md)