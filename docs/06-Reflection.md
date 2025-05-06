---
title: Reflections
---

## **Lessons Learned**
**1) Time Management:** Team 201 quickly learned that managing time is most important. It was necessary that in-class labs were completed days in advance, so progressing through the final project could be as smooth as possible.
  
**2) Adaptability:** The team learned to be very adaptable. Improvising became a skill and changes could be made very quickly when something unpredictable happened.
  
**3) Navigating Datasheets:** Datasheets were referenced almost daily and became an instant referral to when any member had a technical question about a specific component — hence the group learned how to skillfully navigate through many datasheets.
  
**4) Team Dynamics:** The dynamics of Team 201 was something important to understand, and is one of the biggest things the group learned how to harness. By the end of the project, team members understood each other and knew how to handle different tasks and situations effectively.
  
**5) UART Messaging:** Messages received over UART should be recorded into a buffer before messages are handled, rather than handling messages as they come in. This prevents code from blocking incoming UART characters and mishandling messages.
  
**6) Signal Processing:** Signal stability is critical - appropriately sized bypass capacitors should be used on every power pin, and pullup resistors used in I2C/SPI should be appropriately sized to reach a defined square wave rather than a saw-tooth wave. Add some extra space between communication lines to prevent noise such as cross-talk.
  
**7) Power Management:** Power should be isolated and directed appropriately. Each subsystem on every board should be able to be isolated via jumpers. Test points should be placed wherever possible, and fuses should be placed before every critical section. Consider MOSFETs or transistors to isolate the microcontroller from power surges.
  
**8) Component Setup:** Many I2C/SPI components need to be configured at least once - these config buffers require careful reading of the datasheet and an understanding of bit-shifting in order to function properly.
  
**9) Seeking Professional Feedback:** There were plenty of opportunities for the team to receive expert-level feedback (through office hours, external design review, etc.). It was important to capitalize on these moments and obtain the best level of feedback and direction as possible.
  
**10) Non-Blocking Software:** As mentioned in the software changes section, non-blocking code and interrupts lead to more efficient code and smoother operation.  It is better to use slightly more complex code than it is to accept clunky operations that potentially break other features.

## **Recommendations for Future Students**

**1) Implement Modularity:** For those utilizing various serial protocols for the first time, it is highly recommended that a working prototype (perhaps on a breadboard) be developed, so as to ensure a performant final product. Eric and Bradley, who employed I2C and SPI protocols, respectively, both discovered issues with their protocols that, had they been identified on a breadboard earlier, could have been prevented or more speedily addressed.  

**2) Prioritize Power:** For individual PCB’s, the team recommends that the power subsystem is developed first so that shorts or power problems that may occur in further use are avoided. This practice is also crucial when using current-sensitive ICs and when on-board power is used to power other system components (whether on-board or on a teammate’s board).  

**3) Ensure Compatibility:** It is highly recommended that each subsystem’s boards are developed as an isolated individual board before they are combined together. This practice will help the team coordination and planning in order to ensure that each subsystem works independently should one’s system malfunction and need to troubleshoot for errors.  

**4) Consider Environments:** For those with sensors, the team recommends trying to measure and test the devices in a variety of environments to help with calibration. This is due to the fact that the sensor will likely be moved to another location than the one it was initially calibrated in. This practice will help with possible calibration issues due to unknown factors within each environment such as differences in lighting, sounds, spacing, and temperature.  

**5) Establish Language:** For those who use MQTT, discuss the team’s message structure beforehand and ensure that it minimizes the use of backslashes or special characters. This is due to the fact that the MQTT Explorer app used in class does not translate symbols very well into the messages sent over UART or python unless some translating and byte conversion is done.  

**6) Observe Components:** Be mindful of each component's physical properties! These parts are **far** smaller than many realize, and components with sub-millimeter pins will be difficult to install. Make sure you have plenty of room for each component, each pin can be reached individually with a soldering iron, and every part has exposed leads.

## **Version 2.0**
Team 201 acknowledges the potential of the communication structure developed. Future iterations could include additional corrections or improvements to message types or device functionality. In developing a second iteration of the element sorter, Team 201 would seek to improve the timing of message sending, as well as include more control over how messages influence individual board functions. 
  
For example, the current edition of the element sorter detects colors immediately after a selection is made on the HMI. While this was necessary for the current hopper and indexing configuration, greater control and timing of the color sensing would increase the reliability of the color sensor’s reading and lead to more consistent device function. To implement this, one would have to implement an interrupt-based timer within the C-code of the color sensor subsystem and an additional message type that would function as a status message between the various subsystems. The timer and status messages would work together to provide a tuned system response based entirely on user input (where the sensor waits for both the motor and/or HMI to be completely ready for a new ‘element’ ball to be scanned and then send a new accurate result), and would serve to eliminate erroneous scans that have led to incorrect displays on the HMI. 
  
While the subsystem to most utilize this improved function would be the RGB sensor, the functionality could very easily be utilized by the actuator subsystem; an interrupt-based timer would greatly improve UART communication as a byproduct of removing the blocking delay functions currently used by the stepper motor subsystem. To better debug this, Team 201 would further develop the MQTT scoping and message simulation in order to test and debug each individual board’s response to this new functionality.
  
Team 201 recommends adding a debugging status message throughout each subsystem’s code that can be sent every few seconds or on demand as a ‘heartbeat’ function. This would serve to ensure that each program is completely functional or to indicate where any particular program is freezing or generating errors. This could be used in conjunction with the MQTT scope to determine the status of each board during development, design, and refinement.
  
While no simplifications are currently recommended by Team 201, the team recommends the above improvements to simplify device function and debuggability. 
