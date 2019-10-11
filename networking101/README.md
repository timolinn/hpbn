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

At the heart of the internet is the IP and TCP. The IP or Internet Protocol is what provides host-to-host routing and addressing. TCP is what provides abstraction of a reliable network running over an unreliable channel.
