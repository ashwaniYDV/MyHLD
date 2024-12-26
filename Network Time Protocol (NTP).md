# Network Time Protocol (NTP) 
---

### Resources
* https://www.techtarget.com/searchnetworking/definition/Network-Time-Protocol

### About NTP
* NTP is a mechanism that synchronizes the system time of multiple computers over a network. 
* NTP is used to keep a computer's clock accurate by periodically comparing it to a reference server.
* NTP is a built-on UDP, where port 123 is used for NTP server communication and NTP clients use port 1023 (for example, a desktop).
* Unfortunately, like many legacy protocols, NTP suffers from security issues.

#### How it works
* The NTP client requests time from the NTP server, calculates the link delay and local offset, and adjusts its local clock to match the server's clock. 

#### Accuracy
* NTP can maintain time to within tens of milliseconds over the internet, and to within one millisecond in local area networks. 

#### Use cases
* NTP is important for many applications, including databases, clusters, planning and calendaring, and teamwork. 

#### Configuration
* To sync time using NTP, you can install local NTP servers and configure each node with an NTP client. You can also specify multiple servers for redundancy. 

#### Port numbers
* NTP uses port 123 for server communication and port 1023 for clients. 

#### Alternative
* If you need to synchronize clocks, you can also use an internal NTP server that synchronizes the remaining clocks in the internal network.

#### Security
* NTP is vulnerable to security issues, such as spoofing NTP packets, which can cause clocks to be set to incorrect times. 
