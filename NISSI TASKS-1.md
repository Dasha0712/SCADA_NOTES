# Learnings
1. process database - temporary database, it is for storing temporary signals when it is live 
2. report database - it is permanent database, it is for storing the permanent dat like measurement report
3. flow of data coming from ied is(ied(iec61850 server) -> iec 61850 client and opc data access server(CET TOOL) -> OPC DATA ACCESS CLIENT AND COMMUNICATION PROGRAMMING INTERFACE(EXTERNAL OPC DA CLIENT CONFIGURATION TOOL) -> APPLICATION COMMUNICATION PROTOCOL (OR)TCP/IP  PROTOCOL->sys600(process object)    --------------------------> THIS IS for iec61850 protocol

4. for modbus protocol and iec104 protocolm, the flow is MFM -> Sytem configuration -> sys600(process object)

5. Process communication:
The task of the process communication is to form a communication link between the SYS600 system
server and various process devices, like IEDs, RTUs, PLC and so on. The communication with process
devices uses various types of protocols, such as IEC 60870-5-10x, IEC 61850, DNP, Modbus, LON,
SPA and so on. Each protocol has its own characteristics and the physical media and interfaces have
to be built accordingly. The software interface in SYS600 is handled by a communication unit. The
communication unit is dependent on the protocol that is used.
**The most common communication units are the PC-NET and the IEC 61850 OPC Server/External
OPC DA Clien. 
The PC-NET is used with most of the SYS600 protocols, except IEC 61850, which
is handled by the IEC 61850 OPC Server and the External OPC DA Client,** or alternatively, using the
IEC 61850 Client, which supports Edition 2.1 of the IEC 61850 standard. The process communication
can be integrated in the system server in compact systems and it can also be allocated to dedicated
communication servers.

6.  PC-NET
It includes protocol drivers for all supported protocols except those connected with the CPI interface.
These are IEC 61850, IEC 60870-6 (ICCP), and Modbus Slave.

7. why do we create ip address and put it in the scd file creation at opc server

8. how node is been creatred for mfm 

9. When a **PC-NET configuration** is created with the **System Configuration Tool**, the tool produces two
data files: SYSCONF.INI and SIGNALS.INI. When the system is started, it reads the mentioned files
and creates a file PC_NET.CF1 automatically. To create system objects, the System Configuration
Tool automatically creates the file SYS_BASE.SCL, which is executed at system start-up. After the
PC-NET has started, the system executes the file SYS_NET.SCL to configure the PC-NET. The file
is automatically created by the System Configuration Tool. A step-by-step description of the System

Configuration Tool operation is described in Section, PC-NET Startup with System Configuration Tool.
This information is rarely needed, and in practice, the system configuration can be entered and
controlled without knowledge of the internal operation of the tool.


10.   **Configuring process communication**:

The configuration of process communication is divided into the following configuration tasks:
• Configuring communication system objects in base system
• Configuring process communication units (PC-NET, External OPC DA Client, Modbus slave, CPI
applications)

In general, the process communication units cannot be created or accessed from any application before
the corresponding system objects in base system has been configured. The definition method of these
system objects depends on used process communication unit type.
The required base system object types are:
• LINn:B, which defines a communication route from the base system to the defined node.
• NODn:B, which in this context defines the node of process communication unit itself.
• STAn:B, which defines a station within a defined node.
• PRIn:B, which defines a printer within a defined node.
If the used process communication unit is of type PC-NET and System Configuration Tool is used, these
base system objects are created automatically. If the System Configuration Tool is not used, or the used
process communication units are other than PC-NET, see 
Configuring IEC 61850 with External OPC DA
Client and 
Configuring other CPI-connected applications or to PC-NET start-up with SCIL commands in
Configuring PC-NET.
These objects can be also be configured using SYS_BASCON.COM or with SCIL using statement
#CREATE.
Configuring communication system objects in base system
GUID-D63233E5-EC3D-4D0C-9443-6C44713B7C91 V1 en-us
The following system objects are needed for each process communication unit:
Links (LIN)
GUID-4DAD33B7-01C1-40F5-922C-6223B3E176B0 V2 en-us
A link is a data transmission line to another base system, a NET unit or a device. Each link is defined by
a LINn:B object (n = 1 ... 250). A base system can have the following links:
• One link of type LAN. The process communication unit may be directly connected through LAN link.
The LAN link is used between base systems and the process communication unit may be connected
to another base system (indirect connection).
• One link of type INTEGRATED for each configured PC-NET. This link type is used only by PC-NET
process communication unit and it is created by the System Configuration Tool. The PC-NET.EXE
process is started when the LIN:B object of type INTEGRATED is created.
Node (NOD)
GUID-DF04CC3D-C1D9-41BE-93A8-B1B7152BB9FD V2 en-us
Nodes are directly or indirectly connected base systems and process communication units. The nodes
are defined by NODn:B objects (n = 1 ... 250). A node definition is needed for:
• Communication to the communication units. Each process communication unit which is recognized
by the base system must be defined as a node. These node definitions are described in SYS600
System Objects (1MRK511730-UEN) manual.
• Reading and writing attributes. A node is primarily specified by the used connection link and the
station address of the node. If a node is only indirectly connected to the base system, the link to the
node is the link to the nearest intermediate node. The link object must have been defined before the
node can be defined.
Node definition is also used to define another base system in a network.

Example
GUID-2E5782A3-C9DA-43A9-8640-145C3F2718CD V2 en-us
A process communication unit uses LAN link and is configured to be node 3 and its address is 203.
The first step in the configuration is to create a LIN:B object of type LAN (if not yet created for the base
system communication). In this situation, the link number can be selected. The next step is to create
NOD3:B object for the process communication unit and assign selected link number to the NOD3:BLI
attribute of the created object. The node address 203 is assigned to the attribute NOD3:BSA.
This configuration must be present before the process communication unit in node 3 can be accessed.
For more information on system objects of type NOD and LIN, see SYS600 System Objects
(1MRK511730-UEN). The process communication unit is PC-NET and System Configuration Tool is
used, these base system objects are created automatically, otherwise the NOD and LIN need to be
created with SCIL. The example above would then require the following lines:
#CREATE LIN1:B = LIST(LT = "LAN", TR = "TCPIP") 
#CREATE NOD3:B = LIST(LI = 1, SA = 203)
Configuring process communication units
The process communication unit types are listed below:
• PC-NET
• External OPC DA Client (IEC 61850 OPC Server / SNMP OPC Server)
• IEC 61850 Client
• IEC 61850 Server
• IEC 60870-6 (ICCP)
• Modbus slave
• Other CPI-connected applications
GUID-5F1BD128-8FAA-4B28-A39A-7B090411F435 V3 en-us
Most of the communication protocols are supported by the PC-NET process communication unit.
For more information on attribute PO and a complete list of protocols, see SYS600 System Objects
(1MRK511730-UEN). Communication unit of type External OPC DA client is used with OPC connected
protocols such as IEC61850 or SNMP. With all CPI-connected modules, including the applications not
part of SYS600 product, LAN link is used as the communication route between base system and the
communication unit.
Parameters of SNMP OPC Server are described in manual SYS600 Application Design (1MRK511694
UEN). SNMP OPC Server can be configured using Communication Engineering Tool (CET) or with IET
Data Loader.
CDC-II slave is deprecated from version 10.0 since it used special hardware not supported in latest
Windows versions and modern computers.
Configuring PC-NET
GUID-E93D79E8-66F6-413F-A044-BD1A03364DE1 V3 en-us
The recommended way to configure process communication unit of type PC-NET is to use System
Configuration Tool. While creating the full configuration, it provides a set of possible selections in each
step. In practice, these selections are mainly protocol specific line type and station types. It is also
possible not to use the System Configuration Tool and create the line and station configuration using
SCIL. The protocol specific manuals contain examples of how this is done with each protocol. This
method is often used when a MicroSCADA system is updated to a newer release and the amount of
changes to the system is tried to minimize.
When the PC-NET program is started, it reads the initial configuration file PC_NET.CF1, which is a text
file located in the \sc\sys\active\sys_ directory. It defines the basic communication nodes and addresses
to enable the communication to an application that downloads the total configuration.

When a PC-NET configuration is created with the System Configuration Tool, the tool produces two
data files: SYSCONF.INI and SIGNALS.INI. When the system is started, it reads the mentioned files
and creates a file PC_NET.CF1 automatically. To create system objects, the System Configuration
Tool automatically creates the file SYS_BASE.SCL, which is executed at system start-up. After the
PC-NET has started, the system executes the file SYS_NET.SCL to configure the PC-NET. The file
is automatically created by the System Configuration Tool. A step-by-step description of the System
Configuration Tool operation is described in Section, PC-NET Startup with System Configuration Tool.
This information is rarely needed, and in practice, the system configuration can be entered and
controlled without knowledge of the internal operation of the tool.
In case the NET node number is bigger than 50, the default addresses of the MI attributes may overlap
with some other NET nodes and the default values may have to be changed. See the MI attribute
description in SYS600 System Objects (1MRK511730-UEN)manual for details.
Start-up definition file PC_NET.CF1
GUID-ADD7A9C7-DE44-4D01-8B47-62990FDF1610 V1 en-us
When PC-NET process starts, it always reads the start-up configuration file PC_NET.CF1. This file is
generated automatically by the System Configuration Tool. If the configuration is loaded with SCIL, it
may be necessary to edit this file. The following PC_NET.CF1 file is included in the MicroSCADA X
Control System SYS600 delivery as a default configuration:
    local_node.sa=203 ; The station address of the PC-NET. 
    local_node.nn=3 ; The node number of the PC-NET.  
    ext_node(1).sa=209 ; The station address of the base system. 
    ext_node(1).nn=9 ; The node number of the base system.  
    ext_apl(1).nn=9 ; The node number of the base system.  
    ext_apl(1).an=1 ; An application in the base system.
In case the PC_NET.CF1 file is missing when the PC-NET is started, a default configuration becomes
valid.
PC-NET start-up with System Configuration Tool
GUID-204F860A-FE97-496E-B18F-8F20038864A1 V1 en-us
The information given in this section describes the internal operation of the System Configuration Tool.
Usually, this is transparent for the user.
The System Configuration Tool creates procedures for automatic start-up and configuration of the PC
NET. The automatic starting/configuration can be switched on or off. Manual starting/stopping of the
PC-NET can be done in online mode. The automatic starting and configuration of the PC-NET works in
the following way:
• A command procedure SYS_INIT_1:C is connected to the event channel APL_INIT_1:A as the first
secondary object. If the list of the secondary objects is full, the last one is removed and a warning is
generated (notify window, log file).
• The command procedure SYS_INIT_1:C calls a text file (StartPC-NET.SCL) which starts the PC
NET. The program in the text file first updates the sys_/PC_NET.CF1 file and then starts the PC-NET
by setting the corresponding base system link object type to INTEGRATED. The PC_NET.CF1 file is
updated in the following way:
The PC-NET sends a system message to the own application when it is started. This message is
received by a process object to which an event channel, SYS_NET'net_number'D:A, is connected.
This event channel calls a command procedure SYS_NET'net_number'D:C. If the process object
exists (for example created by LIB5xx) and has an event channel connected to it, all the objects
connected to that event channel is moved to the SYS_NET'net_number'D:A event channel as
secondary objects. In other cases, the System Configuration Tool automatically creates a process object
SYS_NETD:P('net_number'), to which the event channel SYS_NET'net_number'D:A is connected.
The command procedure SYS_NET'net_number'D:C checks the message coming from the
PC-NET. If this is the start message (10001), the PC-NET is configured according to the
information entered in the System Configuration Tool.
If the system configuration contains many PC-NETs, then for each of the PC-NET processes to be
started by System Configuration Tool, an instance specific copy of PC_NET.CF1 file can be found from
the same folder. These files follow the file naming convention DEBUG'net_instance_number'.CF1, for
example, file name DEBUG1.CF1 is used for the first PC-NET process instance.
All the possible error messages that occur during the start-up or configuration of the PC-NET are shown
in the notify window. They are logged into the SYS_LOG.CSV log file, which can be viewed using some
text editor of Windows operating system.
PC-NET start-up with SCIL commands
GUID-4C7E8F5F-4B4D-4599-816E-DE7034829D98 V2 en-us
A command procedure for online reconfiguration of PC-NET can be started as follows:
• When a PC-NET unit is restarted, it sends the system message 10001 to all the defined applications
(by default to process object address 6000 + NET no), provided that the application is running. The
system message can be used for updating a process object which activates an event channel, which
in turn starts a command procedure with reconfiguration commands. For more information describing
the System messages from PC-NET units, see 
System messages - base system configuration and
System messages - PC-NET configuration.
• When the connection between PC-NET and an application recovers after a break, PC-NET sends
the system message 1000 + APL no. to the application (by default to address 6050 + NET no.).
This message can be used for conditional start of reconfiguration procedures, that is, reconfiguration
takes place if PC-NET has been restarted, not if the application has been out of use. This can be
checked for example by reading a system object attribute configured online. If online configuration
changes are valid, PC-NET has not been out of operation.
Reconfiguration commands could also, for example, be included in the command procedures started by
the event channels APL_INIT_1 and APL_INIT_2, (APL_INIT_H in hot stand-by systems, see SYS600
Application Objects (1MRK511695-UEN). However, a PC-NET unit can be restarted even though the
application is not restarted.
The protocol specific manuals contains examples for the configuration script for each protocol. In
principle, following step are needed for every protocol:
1. Define the NET line to be used by assigning it the wanted protocol.
2. Give the line its communication properties by means of the line attributes.
3. Create the station(s) by giving it an object number and assigning it the line number.
4. Set the attributes of the created object.
5. Take the line and the device into use.
In SCIL, communication system objects are created and deleted using NET attributes, see SYS600
System Objects (1MRK511730-UEN). When adding a device, the NET line should first be defined. NET
lines are defined by the NET line attribute PO. The used hardware device is generally defined with
attribute SD, which, for example, may refer to certain serial port, PCLTA card or system device.
Defining external nodes (NET)
GUID-E232173D-1EA3-4624-8416-1F55FEFAA466 V1 en-us
All the connected base systems and communication units are defined as external nodes (NET objects).
This applies also to base systems and communication units, which are only indirectly connected via
other communication units.
The primary external node, that is, the PC-NET that communicates via integrated link is defined in
PC_NET.CF1 file and the corresponding attribute values are updated. If there is a need to define other
nodes, the configuration of NET node attribute NE need to be configured.
Defining applications (APL)
GUID-A388092B-A136-4014-AB4E-D0CB2D75E318 V2 en-us
As a rule, all the applications in all base systems, which are directly or indirectly connected to the
communication unit, should be defined to the NET unit as APL objects. The defined applications can
be configured to receive spontaneous messages from the stations and system messages generated by
NET.
In order to define or redefine an application in a connected base system:
1. Define Application, an APL object, in the preconfiguration or online by means of the NETn:SSY
attribute. For more information, see SYS600 System Objects (1MRK511730-UEN).
2. Assign it to the following attributes for the communication supervision. For more information, see
SYS600 System Objects (1MRK511730-UEN).
From SCIL, the SW and SU attributes are accessed as NETn:S attributes. If NETn:SSU value for the
application is non-zero, NET supervises all the application connections by cyclically reading the DS
attribute of all known applications at the interval calculated from the SU attribute. If an application does
not reply, an error message is produced and the application is suspended. This happens when the base
system is closed, when the application has been set to COLD, the application does not exist, or the
connection is faulty or disturbed, or the communication does not work. When an application has been
suspended, the RTUs connected to that application are not polled until the communication with the
application has been re-established. This is the default functionality in attribute SU.
If NETn:SSU has been set to 0 with main application, application connection is never suspended and it
is possible to keep master protocols communicating in the COLD system. This requires that connected
IEDs can serve at least two masters simultaneously. See description of NET node attribute SU for
details. Slave protocol lines in PC-NET node must always be kept out of use in COLD system.
If the defined application is not running in a base system directly connected to the PC-NET but is
running in another node, the NOD:B and LIN:B objects should define the route to the destination node.
This route is usually a LAN link, but this may also be a serial line ACP.
Online configuration changes
GUID-EE687085-9A16-4EED-9FE0-5536ADF90E26 V2 en-us
The online configuration changes can be done in the online mode of the System Configuration Tool, with
SCIL from a Test Dialog or from a command procedure.
The online changes take effect immediately. However, if the PC-NET unit is stopped and restarted,
the online changes are lost and the preconfiguration is restored. Online changes which need to be
permanent, and are not made in the preconfiguration should, therefore, be included in a command
procedure which is executed each time the PC-NET unit is restarted.
When SCIL is used, the attributes are accessed with the object notation according to the format:
OBJnn:Sati
Table 5: Object notation table
'nn'
'at'
'i'
Object number (device number)
Attribute name
The possible index
OBJ in this context may be STA referring to a station object or PRI referring to a printer object. For 

The attributes are written with the #SET command according to the format:
#SET OBJnn:SATi = value
The line attributes can be changed with the SCIL command #SET:
#SET NETnn:Sati = value 
'i' Line number
For detailed information on each attribute, see SYS600 System Objects (1MRK511730-UEN) or protocol
specific configuration manuals.
System messages - base system configuration
GUID-112E2973-B91D-4B47-B7BB-E086099AD8C0 V2 en-us
To use the system messages in an application, follow the instructions given below:
1. Create a fictitious process object of type ANSI analog input and set the Unit Number (UN) attribute to
0. The system message codes of the device is registered as the object value of this object.
2. Set the objects Object Address (OA) attribute equal to the Message Identification (MI) attribute and
set the Switch State (SS) attribute to Auto.
3. Select direct scale (1-1).
Define the consequential operations by using the event, alarm and printout attributes. For more
information on alarm generation, activation of event channel and automatic updating in pictures, for
printout activation and for including event history in the event list, see SYS600 Application Objects
(1MRK511695-UEN).
The default values of MI attributes for each station type are presented in the SYS600 System Objects
(1MRK511730-UEN) manual and in the protocol specific manuals. The defaults listed below are protocol
independent:
Table 6: Message Identification (MI) default value table
Object
Message
NET itself
General messages, for example start-up messages
Value: status code
MI default value
6000 + NET no.
Application supervision
Value: APL no.= failure
1000 + APL no. = recovery (APL no. as known to NET)
NET line
All NET line messages
System messages - PC-NET configuration
6050 + NET no.
6000 + 100 NET no. +
line no.
GUID-7B0F2982-4522-4FE4-831D-53211FB26615 V2 en-us
When a system message is caused by a system object, it is directed to the application specified by the
Message Application (MS) attribute of the object. The code of the message is updated as the object
value for a fictitious process object with the Object Address (OA) attribute value equal to the value of
the Message Identification (MI) attribute. In case the NET node number is bigger than 50, the default
addresses of the MI attributes may overlap with some other NET nodes and the default values may have
to be changed. See the MI attribute description in SYS600 System Objects (1MRK511730-UEN) manual
for details.
To achieve system message handling in the communication unit:
1. Set the Message Application (MS) attribute of the system object to the number of the receiving
application.
2. Set the Message Identification (MI) attribute of the system object to the value of object address
of the receiving object. The MI attribute has object dependent default values which are also the
recommended values, and should generally not be changed. The default value is used when the
system object is defined online and the MI attribute is not explicitly set, or if the MI attribute is set to
0 in the preconfiguration. The default values are shown in the 
Table 6 above, in the SYS600 System
Objects (1MRK511730-UEN) manual and protocol specific manuals.
The transmission of system messages from individual objects can be enabled or disabled by the System
Message Enabled (SE) attribute of the objects. The system message generation should only be disabled
in special cases, for example, if the base system application program often executes commands, which
cause unwanted system messages.
The SE attribute exists for the PC-NET Node and it is accessed by NETx:SSE. This attribute controls the
transmission of the system messages from all objects configured to the node. This attribute can also be
used to enable the updating of the binary status points.
Configuring IEC 61850 with External OPC DA Client
GUID-25E0D994-9E48-4B8D-889F-8F915C30B6F3 V4 en-us
External OPC DA Client and IEC 61850 OPC Server (MMS Client for IEC 61850) are included as a
separate executables in the SYS600 product. External OPC DA Client is communicating to SYS600
base system via LAN link and acting as one communication node in a MicroSCADA network. In other
words, the related base system objects has to be created in the file and the OPC namespace of the IEC
61850 OPC Server has to be mapped into process objects. The required station type for units is SPA.
In case of IEC 61850 OPC server, the .ini file containing the mapping from OPC server items to process
objects is created by SCL Importer tool or IET Data Loader. With other OPC servers, the mapping
between OPC items and process objects can be done using External OPC DA Client Configuration Tool.
The creation of the base system nodes and station objects are supported in System Configuration Tool,
see 
Configuring External OPC DA Client for IEC 61850 OPC Server and other OPC Servers.
For description of the different topologies and configuration details of this IEC 61850 communication,
see SYS600 IEC 61850 System Design (1MRK511715-UEN). Otherwise, the configuration of External
OPC DA Client has been described in SYS600 External OPC Data Access Client (1MRK511703-UEN)
and IEC 61850 OPC Server in SYS600 IEC 61850 Master Protocol (OPC) (1MRK511714-UEN).
Configuring IEC 61850 Client
GUID-273716E9-A759-4D49-8AB6-08E6D9BFED1F V2 en-us
IEC 61850 Client is a separate executable that is connected to the base system via a LAN link. The
configuration of the base system objects described in 
Configuring communication system objects in
base system is needed before communication between the base system and the IEC 61850 Client
executable is established.
For a description of the configuration details of this process communication unit type, see the SYS600
IEC 61850 Client (1MRK511717-UEN) manual. Compared with IEC 61850 OPC Server, this is an
alternative implementation for the IEC 61850 communication to the process devices. SYS600 IEC 61850
Client (1MRK511717-UEN) manual contains a separate section which describes the main differences
between IEC 61850 Client and IEC 61850 OPC Server.
For a description of the different topologies and configuration details of this IEC 61850 communication,
see the SYS600 IEC 61850 System Design (1MRK511715-UEN)manual.
Configuring IEC 61850 Server
GUID-CD4C1361-0A2C-49DA-939C-978C9148DFC7 V3 en-us
IEC 61850 Server is a separate executable which is connected to base system via LAN link. The
configuration of the base system objects described in 
Configuring communication system objects in
base system are needed before the communication between base system and IEC 61850 Server
executable is established.
For description of the configuration details of this process communication unit type, see SYS600 IEC
61850 Server (1MRK511692-UEN)manual
Configuring Modbus slave
GUID-611C49C9-1268-4768-BDC5-85242E24915F V2 en-us
Modbus slave protocol is also supported by a separate executable which is connected to base system
via LAN link. The configuration of the base system objects described in 
Configuring communication
system objects in base system are needed before the communication between base system and the
modbus slave executable is established.
For description of the configuration details of this process communication unit type, see SYS600
Modbus Slave Protocol (1MRK511719-UEN).
Configuring IEC 60870-6 (ICCP)
GUID-6AF9E547-BE56-4252-B4BA-AAD6F11F5AE4 V2 en-us
ICCP is a separate executable which is connected to base system via LAN link. The configuration of
the base system objects described in 
Configuring communication system objects in base system are
needed before the communication between base system and ICCP executable is established. This part
of the configuration is similar whether the ICCP operates as client or server.
Configuring ICCP.
The creation of the base system nodes and station objects are supported in System Configuration Tool,
see 
For description of the configuration details of this process communication unit type, see SYS600 IEC
60870-6 (ICCP) Protocol (1MRK511712-UEN) manual.
Configuring other CPI-connected applications
GUID-8B72C17F-A0EB-4E50-A85B-C47CD8BA9DF8 V3 en-us
CPI (Communication Programming Interface) is a software library for connecting an application to the
MicroSCADA base system. Each application instance using CPI as an interface is seen as node in
SYS600 network and corresponding NODx:B and LINx:B need to be created.
For a description on how the CPI interface is used and how the application instance is seen from the
base system, see SYS600 Communication Gateway, COM500i (1MRK511696-UEN) User's Guide.
Secure authentication using IEC/TS 62351-5
GUID-C72AC29E-4750-4C3D-B4A7-4E6AD1AEE52A V4 en-us


> here i dont know what is node link , explain the detailed manner of two methods of configuring the Configuring process communication


11. 