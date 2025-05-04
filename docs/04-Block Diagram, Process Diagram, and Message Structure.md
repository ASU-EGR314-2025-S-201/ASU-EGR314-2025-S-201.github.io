---
title: Team Block Diagram, Process Diagram, and Message Structure
---

The following diagrams outline the hardware and message structure for Team 201’s project. Team 201’s Block Diagram highlights the various components each team member is using within their individual subsystem as well as the general flow of communication. Following the block diagram is the team sequence diagram. Sequence diagrams are extremely useful in visualizing how messages interact between subsystems. The team discusses the many scenarios/use-cases within this diagram — further details on how each message is structured in their respective situation is the last section of this page. For a comprehensive evaluation of said structure, see Table 4: Message Verification. 

## **Team Block Diagram**
![Team 201 Block Diagram](static/Images/Team_201_Block_Diagram.drawio.png)

## **Team Sequence Diagram**
![UML Diagram](static/Images/Team 201 - UML Sequence Diagram.drawio.png)

Sequence Diagram SVG Download: [link](static/Images/Team 201 - UML Sequence Diagram.drawio.svg)


## **Team Message Format**
UART messages are sent from team member to team member in an 8-byte format. Each byte has a specifix role, outlined in Table 1 below. 

*Table 1: Message Byte Structure*

|**Byte Number**|0-1|2|3|4|5|6-7|
|---|---|---|---|---|---|---|
|**Byte Contents**|AZ|Sender ID|Receiver ID|Message Type|Message Data|YB|

The message prefix AZ and suffix YB function as a message start/stop indicator, allowing team members to efficiently filter out extraneous noise on UART lines as well as anticipate a prescribed message format. Sender and Reciever IDs (see table 2) are used to identify to which team member a message is intended, as well as provide a marker of who sent the message. This is advantageous given the daisy-chain layout of Team 201's project, as messages will be sent from member to member until they are terminated by their recipient or their sender, depending on message type (see Table 3 for Message Type IDs and their respective data).

*Table 2: Team IDs*

| Team member (Role) | Team ID |
|---|---|
|JC <br>(HMI)| H |
|Eric <br>(Sensor)|S|
|Marcus <br>(MQTT Server)|M|
|Bradley <br>(Actuator)|A|
|Broadcast <br>(sends to all members)|X|

*Table 3: Message Types and Data Structure*

| Message Type (UINT8_t) | Message Data info (UINT8_t) |
|---|---|
|**Type 0: Initialization** <br>**Message Type ID:** 0x00|**Message Contents:**<br>0x00 *Enable all systems*|
|**Type 1: Drive Mode** <br>**Message Type ID:** 0x01|**Message Contents:**<br>0x00 *Autonomous Function*<br>0x01 *User-Controlled Function*|
|**Type 2: Sensor Data** <br>**Message Type ID:** 0x02|**Message Contents:**<br>0x00 *Sensor detects Orange*<br>0x01 *Sensor detects Blue*<br>0x02 *Sensor detects Pink*|
|**Type 3: Path Selection** <br>**Message Type ID:** 0x03|**Message Contents:**<br>0x00 *Orient motor Left*<br>0x01 *Orient motor Right*<br>0x02 *Orient motor Right*|

## **Message Verification Table**
The following table (see Table 4) can be used to visualize all message styles to be sent during device function, and how each team member handles various message types. The order in which team members are featured (from left to right) also mirrors the flow of data throughout the device, namely, messages sent from Eric to JC must first pass through Marcus' board, then Bradley's, before finally reaching JC. This functionality is a result of the project's daisy-chain layout.

*Table 4: Message Verification Table*

|Message Type <br>*(Message Type ID)*|JC <br>Role: HMI<br>ID:H|Eric <br>Role: Color Sensor<br>ID:S|Marcus<br>Role: MQTT Server (Web Service)<br>ID:M|Bradley<br>Role: Motor actuation<br>ID:M|
|---|---|---|---|---|
|Initialize Routine<br>*(0x00)*|S<br>(Initialize Systems)|R<br>(Status Light Toggles)|R<br>(Status Light Toggles)|R<br>(Status Light Toggles)<br>(mqtt topic: /EGR314/TEAM201/SUB)|
|Drive Mode Change<br>*(0x01)*|S<br>(Alter Drive Mode to Autonomous or User-Controlled)|-|R<br>(mqtt topic: /EGR314/TEAM201/SUB)<br>S<br>(mqtt topic: /EGR314/TEAM201/PUB)|R<br>(Alter motor positioning algorith based on Drive mode sent)|
|Sensor data<br>*(0x02)*|R<br>(Display Element from detected category)|S<br>(Detected color is _)|R<br>(mqtt topic: /EGR314/TEAM201/SUB)<br>S<br>(mqtt topic: /EGR314/TEAM201/PUB)|R<br>(Alter motor position based on detected color)|
|User path select<br>*(0x03)*|S<br>(Selected path is _)|-|R<br>(mqtt topic: /EGR314/TEAM201/SUB)<br>S<br>(mqtt topic: /EGR314/TEAM201/PUB)|R<br>(Alter motor position based on selected path)|