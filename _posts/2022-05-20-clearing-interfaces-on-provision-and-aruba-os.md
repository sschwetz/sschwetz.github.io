---
typora-copy-images-to: /assets/img
typora-root-url: ..

lang: en-AU
layout: post
title: Clearing Interfaces On Provision and Aruba OS
subtitle: 
tags: [aruba,aruba os,technical,2022,may]
comments: true
redirect_from:
 - /2022/05/20/clearing-interfaces-on-provision-and-aruba-os/
 - /2022/05/20/clearing-interfaces-on-provision-and-aruba-os
---

Whilst troubleshooting a network issue, you may need to clear counters on your AOS or Provision switches.

```
NCB1FGSW01# show int 1/48

  Name  : Crosslink to Switch2                                    
  MAC Address      : d4c9ef-846950
  Link Status      : Up  
  Port Enabled     : Yes    
  Totals (Since boot or last clear) :                                    
   Bytes Rx        : 1,832,003,865        Bytes Tx        : 3,020,588,712       
   Unicast Rx      : 256,594,027          Unicast Tx      : 388,356,220         
   Bcast/Mcast Rx  : 2,468,607,689        Bcast/Mcast Tx  : 913,374,224         
  Errors (Since boot or last clear) :                                    
   FCS Rx          : 0                    Drops Tx        : 2688                
   Alignment Rx    : 0                    Collisions Tx   : 0                   
   Runts Rx        : 0                    Late Colln Tx   : 0                   
   Giants Rx       : 0                    Excessive Colln : 0                   
   Total Rx Errors : 0                    Deferred Tx     : 0                   
  Others (Since boot or last clear) :                                    
   Discard Rx      : 0                    Out Queue Len   : 0                   
   Unknown Protos  : 0                    
  Rates (5 minute weighted average) :
   Total Rx (bps) : 39,960                Total Tx (bps) : 53,855,800          
   Unicast Rx (Pkts/sec) : 3              Unicast Tx (Pkts/sec) : 5            
   B/Mcast Rx (Pkts/sec) : 12             B/Mcast Tx (Pkts/sec) : 6,366        
   Utilization Rx  :     0 %              Utilization Tx  : 05.38 %
```

To clear the interface statistics, the command is `clear statistics <interfaces>` if you wish to clear them all, you can use `clear statistics all`

```
Switch1# show int 1/48

 Status and Counters - Port Counters for port 1/48                    

  Name  : Crosslink to NCB1FB1SW03 48                                     
  MAC Address      : d4c9ef-846950
  Link Status      : Up  
  Port Enabled     : Yes    
  Totals (Since boot or last clear) :                                    
   Bytes Rx        : 43,228,258           Bytes Tx        : 3,855,627,717       
   Unicast Rx      : 46,739               Unicast Tx      : 67,756              
   Bcast/Mcast Rx  : 108,827              Bcast/Mcast Tx  : 57,740,176          
  Errors (Since boot or last clear) :                                    
   FCS Rx          : 0                    Drops Tx        : 0                   
   Alignment Rx    : 0                    Collisions Tx   : 0                   
   Runts Rx        : 0                    Late Colln Tx   : 0                   
   Giants Rx       : 0                    Excessive Colln : 0                   
   Total Rx Errors : 0                    Deferred Tx     : 0                   
  Others (Since boot or last clear) :                                    
   Discard Rx      : 0                    Out Queue Len   : 0                   
   Unknown Protos  : 0                    
  Rates (5 minute weighted average) :
   Total Rx (bps) : 42,128                Total Tx (bps) : 53,909,024          
   Unicast Rx (Pkts/sec) : 3              Unicast Tx (Pkts/sec) : 7            
   B/Mcast Rx (Pkts/sec) : 12             B/Mcast Tx (Pkts/sec) : 6,388        
   Utilization Rx  :     0 %              Utilization Tx  : 05.39 %`
```

