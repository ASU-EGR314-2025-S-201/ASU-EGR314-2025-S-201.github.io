---
title: Team Block Diagram, Process Diagram, and Message Structure
---

The following diagrams outline the hardware and message structure for this project. 

# **Team Block Diagram**
![Team 201 Block Diagram](static/Images/Team-201_Team-Block-Diagram.drawio.png)

# **Team Sequence Diagram**
![UML Diagram](static/Images/Team-201_UML_Diagram.drawio.png)

Sequence Diagram SVG Download: [link](static/Images/Team-201_UML_Diagram.drawio.svg)


# **Team Message Format**
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
|Message Type (UINT8_t)| Message Data info (UINT8_t)|
|---|---|
|**Type 0: Initialization** <br>**Message Type ID:** 0x00|**Message Contents:**<br>0x00 *Enable all systems*|
|**Type 1: Drive Mode** <br>**Message Type ID:** 0x01|**Message Contents:**<br>0x00 *Autonomous Function*<br>0x01 *User-Controlled Function*|
|**Type 2: Sensor Data** <br>**Message Type ID:** 0x02|**Message Contents:**<br>0x00 *Sensor detects Orange*<br>0x01 *Sensor detects Blue*<br>0x02 *Sensor detects Pink*|
|**Type 3: Path Selection** <br>**Message Type ID:** 0x03|**Message Contents:**<br>0x00 *Orient motor Left*<br>0x01 *Orient motor Right*<br>0x02 *Orient motor Right*|


*Table 1: Message Types* 

|Message Type <br> byte 1 <br>(uint8_t) | Description|
|-------------------|---------------|
|0                  | Initialization Message   |
|1                  | Drive Mode    |
|2                  | Sensor Data   |
|3                  | Path Selection|

__________________________________________________________________________

*Table 2: Status Message*  

| Byte 1 (uint8_t) | Byte 2 (uint8_t) |
|---------------------|------------------|
| 0x00                | Initialize all systems            |

Initialization Message Key:  

| Byte 2 (uint8_t) | Description |
|------------------|-------------|
| 0x00             | Initialize     |


__________________________________________________________________________

*Table 3: Drive Mode*

| Byte 1 (uint8_t) | Byte 2 (uint8_t) |
|---------------------|------------------|
| 0x01                | Mode             |

Drive Mode Key:  

| Byte 2 (uint8_t) | Description |
|------------------|-------------|
| 0x00             | Automatic   |
| 0x01             | Direct Drive|

__________________________________________________________________________

*Table 4: Sensor Data*

| Byte 1 (uint8_t) | Byte 2 (uint8_t) |
|---------------------|------------------|
| 0x02                | color            |

Sensor Data Key:

| Byte 2 (uint8_t) | Description |
|------------------|-------------|
| 0x00             | Orange         |
| 0x01             | Blue       |
| 0x02             | Pink        |

__________________________________________________________________________

*Table 5: Path Selection*

| Byte 1 (uint8_t) | Byte 2 (uint8_t) |
|---------------------|------------------|
| 0x03                | path             |

Path Selection Key:

| Byte 2 (uint8_t) | Description |
|------------------|-------------|
| 0x00             | left        |
| 0x01             | center      |
| 0x02             | right       |