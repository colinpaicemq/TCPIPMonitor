# TCPIPMonitor
This repository contains code for z/OS to monitor TCPIP.  It provides

1. [Interface statistics](Interface.md) - periodically it displays the number of bytes etc processed by the interface
2. [Port statistics](Port.md) - bytes processed for connections to a port, along with TCPIP parameers such as window size, and buffer size


## Interface statistics (interface.md)
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

## Port statistics {port}

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