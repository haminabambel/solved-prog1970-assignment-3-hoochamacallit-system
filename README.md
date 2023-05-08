Download Link: https://assignmentchef.com/product/solved-prog1970-assignment-3-hoochamacallit-system
<br>
Create an application suite (a system) that consists of three distinct processing components:

<ul>

 <li>a “data creator” (DC) application – call the application DC</li>

 <li>a “data reader” (DR) application – call the application DR</li>

 <li>a “data corruptor” (DX) application – call the application DX</li>

</ul>

In this system’s development, you will design and implement the data creator application, the data reader application, and the data corruptor application.

The purpose of this assignment is to force all involved in the solution to see how / why and when certain IPC mechanisms are used within UNIX / Linux System Application programming.  That is, what IPC mechanism is better suited for performing a certain type of communication job over another form of IPC.

<h1>The Team</h1>

Since this is an Assignment of considerable size, I do not expect any one individual to complete this assignment on their own – in fact, I suggest that you work together with a partner :

<ul>

 <li>one person can develop both the DC (client application) and the DX (corruptor application)</li>

 <li>one person developing the DR (server application)</li>

</ul>

Before you begin on the assignment, please use the <strong><u>A-03 Group</u></strong> sign-up (under the <em>Groups</em> option in <em>Course Tools</em>) to organize yourselves into partnerships.  Remember that <strong>both members</strong> <strong>need to sign-up</strong> and enroll in the <strong>same group</strong>.  Find a group quickly and start organizing, designing and coding your system!

Please note that it is expected that both members will share equally in the mark given to your partnership’s effort.

<h1>Overall Considerations</h1>

This set of applications will require considerable coordinated effort between the partners.  Here are a couple of other things to consider :

<ul>

 <li>all applications should have modular design</li>

 <li>all applications should have file-header and function comment blocks</li>

 <li>as always – don’t forget the in-line comments within your code</li>

 <li>all code developed in this project must be written in <strong>C </strong></li>

 <li>please hand in your code in the required directory structure with makefiles for each of the DC, DR and</li>

</ul>

DX components o Please see the <u>Linux-Development-Project-Code-Structure</u> document in the Assignment area of eConestoga for information on how to structure the code for a more complex project consisting of multiple binaries

<h1>Data Creator</h1>

<strong>Purpose </strong>

The data creator (DC) program’s job is to artificially generate a status condition representing the  state of the <strong><em>Hoochamacallit</em></strong> machine on your shop’s floor.

<h2>Details</h2>

<ul>

 <li>This application will send a message (via a <strong>message queue</strong>) to the Data Reader application (the server component of the solution).

  <ul>

   <li>Make sure that your DC program follow best practices and checks for the existence of the message queue before sending its first message</li>

   <li>If the message queue doesn’t exist, then the DC application will enter a loop in which it will sleep for 10 seconds and try again to see if the message queue has been established and created</li>

   <li>Only after the message queue has been created can the DC application enter its main processing loop o Once the existence of the queue has been established, the first message that <strong>must be sent</strong> is an <em>Everything is OKAY</em> type message</li>

  </ul></li>

 <li>Each message must contain the machine’s ID value (use the PID of the process)</li>

 <li>As well as a randomly selected status (you’ll need to read up on how the <em>rand()</em> function works) value (except for the 1<sup>st</sup> message) from the list below

  <ul>

   <li>0 : means <em>Everything is OKAY</em> o 1 : means <em>Hydraulic Pressure Failure</em> o 2 : means <em>Safety Button Failure</em> o 3 : means <em>No Raw Material in the Process</em> o 4 : means <em>Operating Temperature Out of Range</em> o 5 : means <em>Operator Error</em> o 6 : means <em>Machine is Off-line</em></li>

  </ul></li>

 <li>this application will send such a message on a random time basis – between 10 and 30 seconds apart</li>

 <li>in making the status value of the message random as well as the frequency of the message itself, this application’s output is not predictable o please note that the DC <strong>should not expect a response or reply message</strong> – all this application needs to do send message after message</li>

 <li>once the DC application has generated and sent a <em>Machine is Off-line</em> message, the DC application may</li>

</ul>

exit o until the Machine is Off-line message is randomly generated and sent, the DC continues to send message after message

<table width="0">

 <tbody>

  <tr>

   <td colspan="3" width="667">while developing the DC application, feel free to have your debug output being printed to the screen.  However,</td>

  </tr>

  <tr>

   <td colspan="2" width="649">when you submit your final version of this application – there must be no output being printed to the screen</td>

   <td rowspan="2" width="18">.</td>

  </tr>

  <tr>

   <td width="380">This application requires no input or output to or from the user.</td>

   <td width="269"> </td>

  </tr>

  <tr>

   <td width="380"></td>

   <td width="269"></td>

   <td width="18"></td>

  </tr>

 </tbody>

</table>

<ul>

 <li></li>

</ul>

<u>NOTE:</u>  It is expected that while running and testing your system solution, you could have multiple DC applications running.  Each DC represents a different Hoochamacallit machine on the shop floor.  There is a maximum of 10 different machines that need to be supported on the shop floor.

<h1>Data Reader</h1>

<h2>Purpose</h2>

The data reader (DR) program’s purpose is to monitor its incoming message queue for the varying  statuses of the different <strong><em>Hoochamacallit</em></strong> machines on the shop floor.  It will keep track of the number  of different machines present in the system, and each new message it gets from a machine, it reports  it findings to a <em>data monitoring</em> log file.   The DR application is solely responsible for creating  its incoming message queue and when it detects that <u>all of its feeding machines</u> have gone off-line, the  DR application will free up the message queue, release its shared memory  and the DR application will  terminate.

<h2>Details</h2>

<ul>

 <li>This application is solely responsible for the creation and destruction of its message queue and shared memory o When the DR application starts:

  <ul>

   <li>It will follow best practices and check for the existence of its message queue, and if not found, will create it</li>

   <li>Also it will create enough space in shared memory for the <em>master list</em> as indicated below</li>

   <li>Then it will sleep for 15 seconds, before beginning its main processing loop (this is to give you time enough to launch some DC clients and have them begin to feed the DR with messages)</li>

  </ul></li>

 <li>The DR will create and maintain a <strong><em>master list</em></strong> of all clients that it has communication contact with o This list will be implemented as a <strong> This structure needs to hold the following data elements</strong>

  <ul>

   <li>The first element needs to contain the message queue ID value that the DR and DC processes are using to communicate</li>

   <li>The second element will represent the number of DC processes currently communicating with the DR</li>

   <li>See the <em>Proposed DR Master List</em> definition at the end of this document o This master list <em>must be implemented in <strong>shared memory</strong></em></li>

   <li>It will be the responsibility of the DR process to create / allocate enough space for this shared memory based upon the key_value gotten with the call</li>

  </ul></li>

 <li>shmKey = ftok(“.”, 16535);</li>

</ul>

o Things to watch out for in this master-list

<ul>

 <li>When the DR recognizes that it hasn’t heard from a DC application for more than 35 seconds, it removes it from the list as assumes that something bad has happened to that DC</li>

 <li>You will need to manipulate the value in element 2 of this list (the number of DC processes currently in communication)</li>

 <li>You will need to remove the DC identifier (and any other information about the DC) from the list of DC clients. When you remove a DC from this list – you will collapse the information of all subsequent DCs to make sure that each remaining DC identifier is in an adjacent element of the list o No blank elements – see example below</li>

 <li>The DR needs to log the removal of a DC in the data monitoring log as a “DC-YY [XXX] removed from master list – NON-RESPONSIVE” (where YY is the 1-indexed ID of the DC in the master list and XXX is the PID of the DC being removed) [NOTE: In all logging messages given below, “YY” represents the 1-indexed ID of the DC and “XXX” represents the PID of the DC]</li>

 <li>With each message that comes in from a DC application, the DR application will perform the following 4 operations inside its main processing loop …</li>

</ul>

<ol>

 <li>Check its master list of machines to see if this new message is from a machine ID that has not been seen before

  <ul>

   <li>If it is new, then it adds the machine ID to its list

    <ul>

     <li>This event is logged as “DC-YY [XXX] added to the master list – NEW DC – Status 0 (Everything is OKAY)”</li>

    </ul></li>

   <li>If this machine is known, it updates the record for this machine in the master list to keep track of the time that this message came in – to monitor the last time the DR has <em>heard from</em> each machine

    <ul>

     <li>This event is logged as “DC-YY [XXX] updated in the master list – MSG RECEIVED – Status</li>

    </ul></li>

  </ul></li>

</ol>

ZZ (aaaaaaaaa)” (where “ZZ” represents the random message sent by the DC and

“aaaaaaaaa” represents the textual description of the status)

<ul>

 <li>In the event that the message status is a value of 6 (meaning the DC has gone off-line), then the DR will instead log the event “DC-YY [XXX] has gone OFFLINE – removing from master-list”</li>

</ul>

<ul>

 <li><strong>No reply message</strong> needs to be <strong>sent</strong> to the sending DC process</li>

</ul>

<ol start="2">

 <li>The DR will check in its master list to see if it has been more than 35 seconds since the last time it heard from <strong><em>any</em></strong> of the machines

  <ul>

   <li>If so, it can assume that that machine is dead and off-line or it exploded (you know those</li>

  </ul></li>

</ol>

Hoochamacallit machines J ) and the DR can remove that machine from its master list

<ul>

 <li>Remember to properly collapse the master-list elements so that adjacent elements in the list still contain valid DC identifiers</li>

 <li>For example : o If your DR is talking to 5 DC processes, your master-list would look like

  <ul>

   <li>Master-&gt;element1 = msgQueueID value</li>

   <li>Master-&gt;element2 = 5</li>

   <li>Master-&gt;DCElements[0] = DC-01 identifier</li>

   <li>Master-&gt;DCElements[1] = DC-02 identifier</li>

   <li>Master-&gt;DCElements[2] = DC-03 identifier</li>

   <li>Master-&gt;DCElements[3] = DC-04 identifier</li>

   <li>Master-&gt;DCElements[4] = DC-05 identifier o Let’s say DC-03 appears to have gone off-line – after properly collapsing the master list it would look like this</li>

   <li>Master-&gt;element1 = msgQueueID value</li>

   <li>Master-&gt;element2= 4</li>

   <li>Master-&gt;DCElements[0] = DC-01 identifier</li>

   <li>Master-&gt;DCElements[1] = DC-02 identifier</li>

   <li>Master-&gt;DCElements[2] = DC-03 identifier [formerly DC-04]</li>

   <li>Master-&gt;DCElements[3] = DC-04 identifier  [formerly DC-05]</li>

   <li>In this case, the client (DC) didn’t say they were going offline, but the server (DR) detected it – in this case the assumption that the DC is dead must be logged with the “non-responsive” logging message noted above.</li>

   <li>Note from the example above that the DC-## is basically given from the DCElements index in the array …</li>

  </ul></li>

</ul>

<ol start="3">

 <li>When the DR application gets an incoming message indicating that a particular machine ID has truly gone off-line, it will remove this machine from its master list (and log an “offline” event as noted above)

  <ul>

   <li>When the number of machines registered in the master list reaches zero

    <ul>

     <li>the DR logs this event (i.e. that all Hoochamacallit machines have terminated and the DR is going to terminate) as “All DCs have gone offline or terminated – DR TERMINATING”</li>

     <li>after logging this final message, the DR will remove its queue and free up its shared memory and exit</li>

     <li>the DR can assume that once a machine has sent an offline message, it will never receive another message from that machine</li>

    </ul></li>

  </ul></li>

</ol>

<ol start="4">

 <li>the final step in the DR’s processing cycle is to sleep for 1.5 seconds before going back to check the message queue for another message</li>

</ol>

<ul>

 <li>Here is a little more information about the formatting of the required DR logging messages – every log event captured is written to a standard ASCII log file – to be found in /tmp/dataMonitor.log o each log entry must start with a timestamp (current date and time)</li>

 <li>You may find <em>asctime()</em> a useful function to work with (prototyped in time.h), along with <em>localtime() </em>as a function to query the system for the current local time

  <ul>

   <li>followed by the required logging event message (to capture what has happened)</li>

   <li>the events to be logged have been indicated in the steps above. Also feel free to log any additional information that you feel would be beneficial within the log file</li>

   <li>an example log entry should appear as follows</li>

  </ul></li>

</ul>

[2018-03-06 21:05:07] : DC-02 [5687] updated in the master list – MSG RECEIVED – Status 3 (Safety Button Failure)

<ul>

 <li>As with the Data Creator – feel free to have any output being printed to the screen while you are developing and debugging this application. But when you submit the code – ensure that there is no output coming from this application.</li>

</ul>

<h1>Data Corruptor</h1>

<h2>Purpose</h2>

The data corruptor (DX) program’s purpose is gain knowledge of the resources and processes involved  in the application suite and then randomly decide between a set of allowable corruptions including :

<ul>

 <li>kill a DC process (to test a DC application going offline in the application suite)</li>

 <li>to delete the message queue being used between the set of DC applications and the DR application If you really think about, the requirements (as listed above) for the DC and DR applications generally describe the <em>happy path</em> through the applications.  The purpose of the DX application therefore is to create the alternative (or exceptions) paths through the DC and DR applications …</li>

</ul>

<h2>Details</h2>

<ul>

 <li>As soon as the DX application is launched, it will try to attach to a piece of shared memory which has been created by the DR process   o the key to use in order to get the shared-memory ID can be gotten from the following call</li>

 <li>shmKey = ftok(“.”, 16535);</li>

 <li>the DX will try to get the shared-memory ID value using this key</li>

 <li>if the shared-memory piece has not yet been created by the DR, then the DX application will sleep for 10 seconds and try again.</li>

 <li>This re-try will continue until either o 100 retries have taken place <strong><em>or</em></strong>

  <ul>

   <li>The DR comes online and creates the shared-memory so that the DX gets the the proper shared-memory ID</li>

   <li><u>it is important that the DX application is running in the same directory as the DR application</u> (because of the parameters used in the ftok() function call)</li>

   <li>this piece of shared-memory will be created and maintained by the DR application. Which means that the DX application will simply be reading the information in this area of shared memory to gain knowledge of necessary information</li>

  </ul></li>

 <li>this area of shared-memory is maintained as a <strong><em>master list</em></strong> of all clients that the DR is in communication contact with</li>

 <li>This list will be implemented as indicated in the <em>Proposed DR Master List</em> definition at the end of this document</li>

 <li>The DX application’s main processing loop will be comprised of the following steps:

  <ul>

   <li>Sleep for a random amount of time (between 10 and 30 seconds)</li>

   <li>When it awakes, the DX should check for the existence of the message queue (between the DC’s and the</li>

  </ul></li>

</ul>

DR)

<ul>

 <li>If the message queue no longer exists, the DX assumes that all of the DC’s have shut down and the DR (having detected this) has exited</li>

 <li>The DX will then log this event as follows, detach itself from shared memory and exit itself</li>

</ul>

[2016-01-06 21:05:07] : DX detected that msgQ is gone – assuming DR/DCs done

<ul>

 <li>Select an action (from the <strong><em>Wheel of Destruction</em></strong> below) randomly (you’ll need to read up on how the <em>rand()</em> function works)</li>

 <li>For the purposes of logging, the <strong><em>Wheel of Destruction</em></strong> will be abbreviated to <strong><em>WOD</em></strong>

  <ul>

   <li>: do nothing</li>

   <li>: kill DC-01 (if it exists)</li>

   <li>: kill DC-03 (if it exists)</li>

   <li>: kill DC-02 (if it exists)</li>

   <li>: kill DC-01 (if it exists)</li>

   <li>: kill DC-03 (if it exists)</li>

   <li>: kill DC-02 (if it exists)</li>

   <li>: kill DC-04 (if it exists)</li>

   <li>: do nothing</li>

   <li>: kill DC-05 (if it exists)</li>

  </ul></li>

</ul>

10: delete the message queue being used between DCs and DR

<ul>

 <li>: kill DC-01 (if it exists)</li>

 <li>: kill DC-06 (if it exists)</li>

 <li>: kill DC-02 (if it exists)</li>

 <li>: kill DC-07 (if it exists)</li>

 <li>: kill DC-03 (if it exists)</li>

 <li>: kill DC-08 (if it exists)</li>

 <li>: delete the message queue being used between the DCs and DR</li>

 <li>: kill DC-09 (if it exists) 19 : do nothing</li>

</ul>

20 : kill DC-10 (if it exists)

<ul>

 <li>Execute the action using whatever system call, IPC call, etc. you need to in order to get the job done</li>

 <li>When “killing” one of the DC processes – make sure to send it a <strong>SIGHUP</strong> signal</li>

</ul>

<ul>

 <li>over and above this main processing loop, the DX application is responsible to logging each action that it takes in a standard ASCII log file – to be found in /tmp/dataCorruptor.log o each log entry must start with a timestamp (current date and time)</li>

 <li>You may find <em>asctime()</em> a useful function to work with (prototyped in time.h), along with <em>localtime() </em>as a function to query the system for the current local time

  <ul>

   <li>followed by some textual description of what has happened</li>

   <li>For example : If action 01 was randomly selected then log a message saying something like :</li>

  </ul></li>

</ul>

[2016-01-06 21:05:07] : WOD Action 11 – DC-01 [7565] TERMINATED




<ul>

 <li>If the DX randomly chose option 10 (the deletion of the message queue), then the DX also logs an additional event (as follows) and then detaches itself from shared memory and exits</li>

</ul>

[2016-01-06 21:05:07] : DX deleted the msgQ – the DR/DCs can’t talk anymore – exiting

<ul>

 <li>Also keep in mind that since this application is really an intruder application – that when trying to kill processes, delete files or remove message queues o proper error checking and handling <strong><em>had better be done</em></strong>. So that this application doesn’t crash! o For example – what if the DX is about to send a “kill” message to a DC, but the DC exits in the meantime … how with the DX handle the failure of the “kill” command?</li>

</ul>