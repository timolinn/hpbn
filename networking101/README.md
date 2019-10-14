# Networking 101

+ Speed is a feature.
+ There are two critical components that dictate the performance of all networks

  + **Latency:** The time from the source sending a packet to the destination receiving it
  + **Bandwidth:** Maximum throughput of a logical or physical communication path

+ Common contributing components for a typical router on the Internet, which is responsible for relaying a message between the client and the server:

  + **Propagation delay:** Amount of time required for a message to travel from the sender to receiver, which is a function of distance over speed with which the signal propagates.

  + **Transmission delay:** Amount of time required to push all the packet’s bits into the link, which is a function of the packet’s length and data rate of the link.

  + **Processing delay:** Amount of time required to process the packet header, check for bit-level errors, and determine the packet’s destination.

  + **Queuing delay:** Amount of time the packet is waiting in the queue until it can be processed.
+ Ironically, it is often the last few miles, not the crossing of oceans or continents, where significant latency is introduced: the infamous last-mile problem.
+ Latency, not bandwidth, is the performance bottleneck for most websites!
+ **Traceroute** is a simple network diagnostics tool for identifying the routing path of the packet and the latency of each network hop in an IP network.
+ On Unix platforms the tool can be run from the command line via traceroute `"(traceroute google.com)"`, and on Windows it is known as tracert.
+ We can improve bandwidth by implementing multiple strategies, one of which is to add more fibre links.
+ However, improving latency is a different story, simply because there is no way around the laws of physics, given that we cannot make light faster than it is already. Since optic fibre cables already transport data at the speed of about 200, 000, 000 metres per second. That is ~2/3 the speed of light, as engineers the best we can do to improve the performance of our applications is to architect and optimize our protocols and networking code with explicit awareness of the limitations of available bandwidth and the speed of light: we need to reduce round trips, move the data closer to the client, and build applications that can hide the latency through caching, pre-fetching etc.

## Building blocks of TCP (Transmisson Control Protocol)

+ At the heart of the internet is the IP and TCP. The IP or Internet Protocol is what provides host-to-host routing and addressing. TCP is what provides abstraction of a reliable network running over an unreliable channel.
+ TCP hides most of the complexity of network communication from our applications: retransmission of lost data, in-order delivery, congestion control and avoidance, data integrity, and more.
+ All TCP connections begin with a three-way handshake: SYN, SYN ACK, ACK
+ The delay imposed by the three-way handshake makes new TCP connections expensive to create, and is one of the big reasons why connection reuse is a critical optimization for any application running over TCP.
+ TCP Fast Open (TFO) is a mechanism that aims to eliminate the latency penalty imposed on new TCP connections by allowing data transfer within the SYN packet.
+ **Flow control** is a mechanism to prevent the sender from overwhelming the receiver with data it may not be able to process — the receiver may be busy, under heavy load, or may only be willing to allocate a fixed amount of buffer space.
+ **1 network segment is 1,460 bytes**
+ It is important to recognize that TCP is specifically designed to use packet loss as a feedback mechanism to help regulate its performance. In other words, it is not a question of if, but rather of when the packet loss will occur. Slow-start initializes the connection with a conservative window and, for every roundtrip, doubles the amount of data in flight until it exceeds the receiver’s flow-control window, a system-configured congestion threshold (ssthresh) window, or until a packet is lost, at which point the **congestion avoidance** algorithm (Figure 2-3) takes over.
+ If you have ever wondered why your connection is transmitting at a fraction of the available bandwidth, even when you know that both the client and the server are capable of higher rates, then it is likely due to a small window size: a saturated peer advertising low receive window, bad network weather and high packet loss resetting the congestion window, or explicit traffic shaping that could have been applied to limit throughput of your connection.
+ TCP three-way handshake introduces a full roundtrip of latency.
+ TCP slow-start is applied to every new connection.
+ TCP flow and congestion control regulate throughput of all connections.
+ TCP throughput is regulated by current congestion window size.
+ With the latest kernel in place, it is good practice to ensure that your server is configured to use the following best practices:
  + **Increasing TCP’s Initial Congestion Window:** A larger starting congestion window allows TCP to transfer more data in the first roundtrip and significantly accelerates the window growth.
  + **Slow-Start Restart:** Disabling slow-start after idle will improve performance of long-lived TCP connections that transfer data in periodic bursts.
  + **Window Scaling (RFC 1323):** Enabling window scaling increases the maximum receive window size and allows high-latency connections to achieve better throughput.
  + **TCP Fast Open:** Allows application data to be sent in the initial SYN packet in certain situations. TFO is a new optimization, which requires support both on client and server; investigate if your application can make use of it.
+ The combination of the preceding settings and the latest kernel will enable the best performance — lower latency and higher throughput — for individual TCP connections.
+ No bit is faster than one that is not sent; send fewer bits.
+ We can’t make the bits travel faster, but we can move the bits closer.
+ TCP connection reuse is critical to improve performance.

### TCP Performance Checklist

+ Optimizing TCP performance pays high dividends, regardless of the type of application, for every new connection to your servers. A short list to put on the agenda:
+ Upgrade server kernel to latest version.
+ Ensure that cwnd (congestion window) size is set to 10.
+ Ensure that window scaling is enabled.
+ Disable slow-start after idle.
+ Investigate enabling TCP Fast Open.
+ Eliminate redundant data transfers.
+ Compress transferred data.
+ Position servers closer to the user to reduce roundtrip times.
+ Reuse TCP connections whenever possible.
+ Investigate "TCP Tuning for HTTP" recommendations.
