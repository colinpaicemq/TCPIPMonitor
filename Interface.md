# Interface statistics
Creates real time statistics for TCPIP Interfaces on z/OS.  It shows what happened in the previous n seconds.   The TCPIP statistics are cumulative.   This program calculates the delta values from the previous time.

You can download the output files to your PC, and us a spread sheet to display the data in a graphical format.


## Example output
```
Interface       ,HH:MM:SS,IBytes,OBytes,IB/sec,OB/sec,,IUP   ,OUP   ,, IMP, OMP, IBP, OBP, InE, OuE, InD, OuD 
TAP0            ,09:11:12,     0,     0,      ,      ,,     0,     0,,   0,   0,   0,   0,   0,   0,   0,   0, 
TAP0            ,09:11:22,     0,     0,     0,     0,,     0,     0,,   0,   0,   0,   0,   0,   0,   0,   0, 
TAP0            ,09:11:32,     0,     0,     0,     0,,     0,     0,,   0,   0,   0,   0,   0,   0,   0,   0, 
TAP0            ,09:11:42,   348,   348,    34,    34,,     3,     3,,   0,   0,   0,   0,   0,   0,   0,   0, 
TAP0            ,09:11:52,     0,     0,     0,     0,,     0,     0,,   0,   0,   0,   0,   0,   0,   0,   0, 
TAP0            ,09:12:02,     0,     0,     0,     0,,     0,     0,,   0,   0,   0,   0,   0,   0,   0,   0, 
TAP0            ,09:12:12,     0,     0,     0,     0,,     0,     0,,   0,   0,   0,   0,   0,   0,   0,   0, 
TAP0            ,09:12:22,     0,     0,     0,     0,,     0,     0,,   0,   0,   0,   0,   0,   0,   0,   0, 
TAP0            ,09:12:32,     0,     0,     0,     0,,     0,     0,,   0,   0,   0,   0,   0,   0,   0,   0, 
TAP0            ,09:12:42,     0,     0,     0,     0,,     0,     0,,   0,   0,   0,   0,   0,   0,   0,   0, 
```

The two entries, Input Bytes per second,and OutputBytes a second are rates per second.  The other values are deltas from the previous time interval.

The report shows 348 bytes received in the time interval.  The time interval is 10 seconds, so the rate is 348/10 = 34 bytes per second.

Where the fields are

Interface
: The 16 character value

HH:MM:SS 
: The time the data was written

IBytes
: The number of Input Bytes received in the time interval

OBytes
: The number of output bytes sentin the time interval

IB/Sec
: The number of Input Bytes/time received in the time interval/duration

OB/Sec
: The number of Output Bytes/time sent in the time interval/duration

IUP
: The number of Input Packets to a specific address (Unicast)

OUP
: The number of Output Packets to a specific address (Unicast)


IMP 
: The number of Multicast Input Packets 

OMP
: The number of Multicast Output Packets 

IBP
: The number of Input Packets addressed to a Broadcast address

OBP
: The number of Output Packets addressed to a Broadcast address

InE
: The number of Input packets discarded due to an error validating the packet

OuE
: The number of Output packets discarded due to an errors

InD
: The number of Input packets discarded due to an out-of-storage condition

OuD
: The number of Output packets discarded due to an out-of-storage condition.


## Output files
The output from the first interface is written to //INTF1 etc, the second to //INTF2 etc.  
If these are not defined the output is written to //SYSPRINT

In //SYSPRINT is information about the interface (written the first time through) for example
```
Interface LOOPBACK         type: Loopback                                     
  Device status: Active                                                       
  Interface status: Active                                                    
Interface LOOPBACK6        IPV6 type: Loopback                                
  Device status: Active                                                       
  Interface status: Active                                                    
Interface TAP0             type: OSA-Express QDIO ethernet OSD                
  Device status: Active                                                       
  Interface status: Active                                                    
Interface TAP1             type: OSA-Express QDIO ethernet OSD                
  Device status: Active                                                       
  Interface status: Active                                                    
Interface JFPORTCP6        IPV6 type: OSA-Express QDIO ethernet OSD           
  Device status: Not active                                                   
  Interface status: Not active                                                
```

It shows if the interface is IPV6, gives its type, and status.

## JCL to execute the program

```
//COLINC5    JOB 1,MSGCLASS=H 
//* The LOADLIB must be APF authorised 
// SET LOADLIB=COLIN.TCPMON.LOAD
//RUN      EXEC PGM=TCPMON,REGION=0M,PARMDD=MYPARMS 
//STEPLIB  DD DISP=SHR,DSN=&LOADLIB 
//SYSPRINT DD SYSOUT=* 
//SYSOUT   DD SYSOUT=* 
//SYSERR   DD SYSOUT=* 
//INTF1    DD SYSOUT=* 
//INTF2    DD SYSOUT=* 
//INTF3    DD SYSOUT=* 
//INTF4    DD SYSOUT=* 
//INTF5    DD SYSOUT=* 
//INTF6    DD SYSOUT=* 
//INTF7    DD SYSOUT=* 
//INTF8    DD SYSOUT=* 
//INTF9    DD SYSOUT=* 
//MYPARMS DD * 
 / 
  --SLEEP 10 
  --COUNT 10 
/*
 --TCPIP TCPIP
```

## Parameters
Where the parameters are (in upper case):

--SLEEP nnn
: How many seconds to sleep between iterations.  It defaults to 60 seconds.

--COUNT nnn
: How many times to iterate it defaults to 50.

--TCPIP
: The name of the TCPIP stack to use - defaults to TCPIP



## Installation
FTP the load/COLIN.TCPMON.LOAD.XMIT to z/OS.  For example using
```
site quote  cyl pri=1 recfm=fb blksize=800 lrecl=80
bin
put COLIN.TCPMON.LOAD.XMIT 'myid.TCPMON.LOAD.XMIT'
```
then use
```
tso receive indsn('myid.TCPMON.LOAD.XMIT')
``` 
You can then copy the load module to an APF authorised library, or APF authorise the library using a command like

SETPROG APF,ADD,DSN=myid.TCPMON.LOAD.XMIT,VOL=......

# Change history
2024 Feb 24: Version 1.0.  Original version


