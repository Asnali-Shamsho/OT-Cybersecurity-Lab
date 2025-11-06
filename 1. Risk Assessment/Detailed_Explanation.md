## Detailed Explanation – Risk Assessment and Network Architecture

This Process is guided by the following standard:
1. **ISA/IEC 62443-3-2** – *Security Risk Assessment for System Design*

The risk assessment follows a structured process consisting of the following steps:
1.  Identification of the System under Consideration (SuC)
2.  Partition the SuC into zones and conduits
3.  Risk Analysis
4.  Risk Evaluation
5.  Risk Treatment

### 1: SuC Identification
The OT Cybersecurity & Process Automation Lab simulates a real-world Industrial Control System (ICS) environment designed to evaluate and test cybersecurity controls and process automation functions in a controlled setting.
The system integrates both cybersecurity management and process control components to reflect an operational technology (OT) environment. It includes:
1.  Primary Domain Controller: *Centralized identity and access management for user authentication and policy enforcement.*
2.  Trellix ePO & ENS: *Endpoint protection and centralized security management.*
3.  Veritas Backup & Recovery System: *Data protection and restoration of critical OT servers.*
4.  Windows Server Update Services (WSUS): *Patch management for controlled updates within the OT domain.*
5.  Enteprise SCADA: *Supervisory Control and Data Acquisition.*
6.  Foxboro Distributed Control System (DCS): *Process Control System.*
7.  Triconex Safety Instrumented System (SIS): *Safety-critical automation ensuring process integrity.*
8.  Programmable Logic Controllers (PLCs): *Localized field control and signal acquisition.*

   #### 1.1 Asset List

![Asset List](./Assets.png)

#### 1.2 System Components, Functions and Data Flow 
1. *Primary Domain Controller (PDC) Server*
   a. Utilizes Active Directory Domain Services (AD DS) for centralized user authentication, group policy management, and access control for all domain clients(EPO       Server, 10EWS1 Server, WSUS Servers, and Backup Server).
   b. Communicates with domain clients over the Auxiliary Control Network (ACN).
2. *EPO Server*
   a. Runs Trellix ePolicy Orchestrator (ePO) for centralized security policy management and monitoring of Trellix ENS clients across zones.
   b. Communication with ENS clients occurs via the ACN.
3. *10EWS1 Server*
   This is the Engineering Workstation (Server) which will:
   a. Hosts Foxboro DCS Controller (FDC280) and acts as the Galaxy Repository Server
   b. Includes Foxboro Control Software for logic design and visualization using HMI.
   c. Equipped with three NICs:
      i. Two NICs for Foxboro Control Network (FCN) communication with FDC280.
      ii. One NIC for communication with PDC, EPO, and WSUS and BAC1 via ACN.
4. *WSUS 2 Server*
   a. Functions as the upstream Windows Server Update Services (WSUS) server, pulling security and system updates from Microsoft Update (Internet or corporate            source).
   b. Has two NICs:
      i. One for Internet access via Enterprise Network (EN)
      ii. One for communication with WSUS Server via ACN.
5. *WSUS Server*
   a. Acts as the downstream WSUS server, receiving approved patches from WSUS 2 to prevent direct Internet connectivity between OT and IT layers.
   b. Distributes patches to EPO, PDC, and Backup servers over ACN.
6. *Backup Server*
   a. Provides backup and restoration capabilities for critical servers and configurations.
   b. Communicates with PDC, EPO, and 10EWS1 servers via ACN.
7. *Field Device Controller (FDC280)*
   a. Serves as the Foxboro DCS control processor, exchanging data with PLCs using Modbus TCP protocol.
   b. Executes control logic to maintain process conditions.
   c. Equipped with redundant NICs for FCN communication with the host server and additional two NICs for field device integration with third part devices using          protocols such as Modbus TCP and OPC UA.
8. *Tricon CX*
   a. Serves as a Safety Controller for the Safety Instrumented System and the Logic will be processed independent of Process Control System.
   b. Equiped with Tripple Modular Redudant (TMC) Controller for Logic Processing and Redudnt Communication Module for communication.
   c. Communicate with Process Control System (Foxboro DCS) through Safety Maintenance Network (SMN).
10. *Enteprise SCADA*
    a. This is the SCADA for high level monitoring and control.
    b. SCADA will be located at Enteprise Layer and communicate with OT System through a firewall.
11. *PLC*
   a. Represents the field controller, communicating with FDC280 via Modbus TCP through the firewall.
   b. Connects to physical field devices using I/O cables.
12. *Switches*
   a. Enable device connectivity and enforce network segmentation.
13. Firewall
    a. Segregates IT, DMZ, and OT zones, enforcing traffic filtering and logging for secure communication
