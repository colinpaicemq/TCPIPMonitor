# Port statistics
Displays information about connections to a port.  It displays TCPIP parameters such as window size and buffer size, which affect the performance of a session.

## Example output
### SYSPRINT
This gives the transmission rates in Bytes In, Bytes out, Segments in, Segments out, Bytes In per second, Bytes Out per second, and retransmit count.
It provides this information every --sleep seconds, for --count times
```
Remote IP V6 2001:7::.                                                                   
Using --PORT 22 --SLEEP 10 --COUNT 3 --TCPIP TCPIP                                       
HH:MM:SS,remote              ,BytesI,BytesO,,SegsI , SegsO,,BI/Sec,BO/Sec,,ReXmtC,       
17:09:50,2001:7::3:43736     ,   156,  1164,,    14,    11,,    15,   116,,     0,       
17:10:00,2001:7::3:43736     ,   624,  3020,,    35,    23,,    62,   302,,     0,       
17:10:10,2001:7::3:43736     ,   676,   708,,    26,    13,,    67,    70,,     0,       
```
### Info
The first time a connection is seen, it displays information about the connection.
```
2001:7::3:43736  JOBNAME SSHD4     Interface JFPORTCP6                                                               
2001:7::3:43736       ResourceId:    42           InSegs:    48          OutSegs:    35         SSThresh: 65535      
2001:7::3:43736      OutBuffered:     0       InBuffered:     0                                                      
2001:7::3:43736       ReXmtCount:     0    CongestionWnd: 48552    RoundTripTime:     2     RoundTripVar:     4      
2001:7::3:43736       SndBufSize: 65536           SndWnd: 64128        MaxSndWnd: 64896          SendMSS:  1440      
2001:7::3:43736       RcvBufSize: 65536           RcvWnd:131020  Lcl0WindowCount:     0  Rmt0WindowCount:     0      
2001:7::3:43736                                                                                                      
```
### Change
If the TCPIP information changes, it reports the before and after values
```
17:09:50 2001:7::3:43736  NWMConnCongestionWnd n:64260 - o:48552 = 15708                
17:09:50 2001:7::3:43736  NWMConnRoundTripVar n:2 - o:4 = -2                            
17:10:00 2001:7::3:43736  NWMConnCongestionWnd n:65688 - o:64260 = 1428                 
17:10:10 2001:7::3:43736  NWMConnRoundTripVar n:1 - o:2 = -1                            
```
At 17:09:50 for the session, 

- the Connection Congestion Window changed from the old value o:48552, to the new value n:64260, a delta of 15708
- The Connection Round Trip variance changed from the old value o:4 to the new value n:2 a difference of 2.

## JCL to execute the program
The load library must be APF authorised.

```
//COLINCPO   JOB 1,MSGCLASS=H,COND=(4,LE)
// SET LOADLIB=COLIN.TCPMON.LOAD
//RUN      EXEC PGM=MONPORT,REGION=0M,PARMDD=MYPARMS 
//STEPLIB  DD DISP=SHR,DSN=&LOADLIB 
//SYSPRINT DD SYSOUT=*,DCB=(LRECL=200) 
//SYSOUT   DD SYSOUT=* 
//SYSERR   DD SYSOUT=* 
//INFO     DD SYSOUT=* 
//CHANGE   DD SYSOUT=* 
//MYPARMS DD * 
 / 
  --SLEEP 10 
  --COUNT 50 
  --PORT 21 
  --IP 2001:db8::/200 
                                                                       
/* 
  --TCPIP TCPIP
```

## Parameters
Where the parameters are (in upper case):

--SLEEP nnn
: How many seconds to sleep between iterations.  It defaults to 60 seconds.

--COUNT nnn
: How many times to iterate it defaults to 50.

--TCPIP TCPIP
: The name of the TCPIP stack to use - defaults to TCPIP

--PORT  nnn
: The port on the z/OS system

--IP address
: The IP address (range) for the remote end of the connection.  For example 
10.1.1.1, 192.168.1.0/8,  2001:1::/64



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
2024 April 22: Version 1.0.  Original version

