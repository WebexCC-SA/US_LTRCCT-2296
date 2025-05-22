---
#icon: material/folder-open-outline
icon: material/bullseye-arrow
---

## Welcome Final Challenge Mission!

## Story
In this short troubleshooting task you're are going to play a technical engineer who got a request from end customer regarding broken production flow. You as an engineer must fix the reported issue before contact center opens in just 20 mins. </br>
So <span style="color: red;">**NO PRESSURE HERE AT ALL!!!**</span>

## Problem Description
A customer reports a critical issue with their call flow: callers cannot land in the queue and are being redirected to the TAC Service number, which is not an intended outcome. Initially, the problem seemed to be related to the queue configuration, but after the customer attempted some adjustments, the situation worsened. Now, callers cannot even reach the queue node, and it seems like the HTTP request has been broken completely.


## Mission Details
Your task is to identify and fix the issues causing this behavior. Specifically:

1. The flow should correctly execute the HTTP request and retrieve the expected value of Queue_140 **ID**.</br>
2. The queue node should reference the queue dynamically using a variable, ensuring that calls are directed to Queue_140.</br>
3. Calls should land in the intended queue instead of being redirected to the TAC Service number.</br>
4. The phone in the middle of the room should ring, signaling a successful fix.</br>

## Before you start

1. Assign **<span class="attendee-id-container">FinalBoss_Flow_<span class="attendee-id-placeholder" data-prefix="FinalBoss_Flow_">Your_Attendee_ID</span><span class="copy" title="Click to copy!"></span></span>** to your **<span class="attendee-id-container"><span class="attendee-id-placeholder" data-suffix="_Channel">Your_Attendee_ID</span>_Channel<span class="copy" title="Click to copy!"></span></span>**.

     ![Profiles](../graphics/Lab2/Tshoot_ChannelAssign.gif)

2. Open the **<span class="attendee-id-container">FinalBoss_Flow_<span class="attendee-id-placeholder" data-prefix="FinalBoss_Flow_">Your_Attendee_ID</span><span class="copy" title="Click to copy!"></span></span>** and you're ready to start troubleshooting.

3. Use the following URL for unauthenticated API requests: ***https://674481b1b4e2e04abea27c6e.mockapi.io/flowdesigner/Lab/DynVars?dn={{NewPhoneContact.DNIS | slice(2) }}***<span class="copy-static" data-copy-text="https://674481b1b4e2e04abea27c6e.mockapi.io/flowdesigner/Lab/DynVars?dn={{NewPhoneContact.DNIS | slice(2) }}"><span class="copy" title="Click to copy!"></span></span>


## Competition & Rewards
A real IP phone is placed in the middle of the room, and an agent logs into Webex Desktop using that phone as the telephony option. Once the call flow is successfully fixed, the phone will ring, and the agent will accept the call.</br>
The first three participants to successfully make the phone ring—without cheating and by following the rules explained previously—will win a prize. This encourages fast and accurate troubleshooting, making the exercise more engaging and competitive.

---

This exercise will help attendees practice debugging and resolving common Webex Contact Center API integration issues. Ensure they document their troubleshooting steps to reinforce learning.


<script src='../template_assets/load.js'><script>