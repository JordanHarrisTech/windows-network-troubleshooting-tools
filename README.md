# Operating System Tools

Some tools that analyze conneciton issues and view client device parameters, settings and cabailities include `ping`, `traceroute`, `pathping`, `nslookup`, `netstat`, and `netsh` (win32). 

- [__Ping__](https://technet.microsoft.com/en-us/library/cc940091.aspx) - most OSes and switches/routers have this feature. Ping is used to attempt an Internet Control Message Protocol (ICMP) communication with a remote host based on the IP address. The sendor sends an ECHO ICMP message (TYPE 8 ICMP message) to the target IP address. If the target IP address both receives the request and is configured to allow responses, it will send back an ECHO REPLY ICMP message (TYPE 0 ICMP message). See [RFC 792](https://tools.ietf.org/html/rfc792) for more detail. 
  + In Windows, two important parameters for testing are `-t` and -`l` The `-t` parameter is used to specify that the operation should run until interrupted (with `CTRL + C`). 
  + The `-l` parameter is used to change the data size in the ECHO message (the sent message) and therefore in the ECHO REPLY message. This function is useful when you want to force more data through the network, which cna reveal problems that a small 32 byte message (the Windows default size) will __not__ reveal. 
- [traceroute](https://support.microsoft.com/en-us/help/314868/how-to-use-tracert-to-troubleshoot-tcp-ip-problems-in-windows) (`tracert`) - this is similar to ping, but different in that it sends ICMP ECHO messages to each node along the path to a destination.
  + This is done with "creative" use of the time-to-live (TTL) field in the IP packet. 
    + First, the command sends three ICMP ECHO messages to the "ping" target with a TTL of 1. Therefore, when the first router receives it, it sends back a TTL Timeout message and, of course, this means the traceroute command now knows that router's address. 
    + Second, the command sends three more ICMP ECHO messages with a TTL of 2. The result, is that the next router in the path receives the packets, but the TTL will be 0, and it therefore responds with a TTL Timeout message. Now traceroute now knows that IP address. 
    + Third, the process continues until tAddhe target is reached. 
  + The benefit of traceroute is that it checks each device along the path. Assuming all routers are configured to respond to ICMP ECHO messages with ICMP ECHO REPLY messages, the traceroute command will help you ensure availability of all routers along the path. Note that on the Internet, it is not uncommon to see request timeout errors from some nodes along the path. Some organizations disable ICMP ECHO REPLY messages for performance and security reasons.
- [__pathping__](https://technet.microsoft.com/en-us/library/cc958876.aspx) - this is an enhance implementation of traceroute in Windows. It determines the route, and also responds with useful statistics about the performance along the path. The pathping command sends ICMP ECHO messages to each hop in the same manner as traceroute and then sends multiple ICMP ECHO messages to each hop to calculate performance over time for each hop. 
- [__NSLookup__](https://technet.microsoft.com/en-us/library/cc940085.aspx) - this is used to query DNS servers. It is useful to use when clients cannot resolve host names to IP addresses or when DNS is intended to be used for location services.
- [__Netstat__](https://technet.microsoft.com/en-us/library/bb490947.aspx) - this is used to show statistics for network connections. For example, running Netstat with an interval in seconds will show active connections and new connections that are created. This can be used to analyze tarets for TCP sessions on the network. 
- [__Netsh__](https://technet.microsoft.com/en-us/library/bb490939.aspx) - this is the network shell command. It reveals many things about network connections and configurations on a Windows machine. Unlike most Command Prompt comands, NETS has different modes with different commands in those modes. 
  + Useful NETSH WLAN commands include:
    + SHOW INTERFACES - reveals current profiles operation, authentication and key management (AKM) protocol, encryption method, channel, signal strength, and data rates. 
    + SHOW NETWORKS - provides informaiton about visible networks that the client station (STA) cna see. 
      + NETSH WLAN SHOW NETWORKS MODE=BSSID - to get more information about a network.
    + SHOW DRIVERS - this will reveal driver files used, security methods provided by the adapter, and radio PHYs supported
    + SHOW PROFILES - useful for evaluating the profiles installed/configured on the local machine.
      + NETSH WLAN SHOW PROFILES NAME="OFFICE24" - this will reveal additional information about the specified profile. 
      + KEY=clear - if you would like to see the stored key. 
  + Additional netsh commands include:
    + NETSH WLAN SHOW ALL
    + NETSH INTERFACE IPV4 SHOW ADDRESSES
    + NETSH INTERFACE IPV4 SHOW IPSTATS
    + NETSH INTERFACE IPV4 SHOW CONFIG
    + NETSH INTERFACE IPV4 SHOW ICMPSTATS
    + NETSH INTERFACE IPV4 SHOW TCPSTATS
    + NETSH INTERFACE IPV4 SHOW TCPCONNECTIONS
  
