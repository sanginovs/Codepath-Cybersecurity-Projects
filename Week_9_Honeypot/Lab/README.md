## Network Config

## Mac: ifconfig (Linux: IP)

* Facts
  * List information about available network interfaces (NICs)
  * Can start/stop individual interfaces
  * ifconfig can change an interface's MAC address

* Challenges
  * Run ifconfig/ipconfig/ip and determine the name/id of your primary network interface
     * After running **ifconfig**, I discovered that there's several network interfaces on my Mac. Some of them are physical network interfaces while others are logical:
         * lo0: loopback interface. localhost 127.0.0.1
         * en0: physical network interface: ethernet
         * en1: physical network interface for WIFI
         * p2p0: virtual interface for Airdrop Mac-specific
         * fw0: FireWire network interface
         * utun0: for VPN MacOS Sierra or later
         * gif0: tunnel interface, used to tunnel IPv4 traffic to Ipv6 network and back
         * stf0: is an IPv6 to IPv4 tunnel interface, basically a bridge connection to transition IPv4 to Ipv6:  http://en.wikipedia.org/wiki/6to4

  * What is your primary interface's IP address? Is it different from your public IP? Why or why not?
     * On en0, it shows that my primary ip **inet** is 10.8.80.XYZ. Since I am connected to ethernet, my laptop is assigned a private IP address. A private IP address is assigned to every node connected to a local network. The organizations that distribute IP addresses to the world reserved a range of IP addresses for private networks. They are:
        * 192.168.0.0 - 192.168.255.255 (65536 IP addresses)
        * 172.16.0.0 - 172.31.255.255 (1,048,576)
        * 10.0.0.0 - 10.255.255.255 (16,777,216)
     * If the node from this network makes a request to a server outside of this network, my router assigns a public IP address to this node and makes a request using a node's public IP. To translate a public IP back to private or vica versa, the router uses Network address translation (NAT). As a result, a private IP address is used only inside local network and my router. That is why I can't see my public IP using **ifconfig** since my router is the one that assigns a public IP to it.

  * What is the MAC address of your primary interface?
     * My en1 MAC address from the **ifconfig** output is **ether xy:xy:xy:xy:xy:xy**. A media access control address (MAC) of a device is a unique identifier for a network interface. It tells who you are while IP address tells where you are. MAC addresses are Layer-2 addresses used to deliver layer-2 frames on a LAN while IP addresses are layer-3 addresses looked at by layer-3 devices such as a router. there are many protocols in Layer 2 some of which make use of MAC addresses. MAC addresses allows us to do several things:
        * Identify manufacturer of an item
        * They provide an unchanging identification of an item
        * Allows for less hardware intensive packets transfer in LAN.
  * Identify and understand your loopback interface
     * virtual, logical interface. 127.0.0.1 is a commonly used loopback ip address. It is a set of ip addresses reserved by IEEE that starts from 127.0.0.1 all the way to 127.0.0.8. It is used to faciliate a client and server communication which are running on the same computer. By using loopback address, a client and server do not need to know the exact IP address of one another. So, they can communicate using generic IP 127.0.0.1. This is super useful especially when you are developing on your local machine and you don't need to setup a server and host your application to test it. **Localhost** is a local server that allows you to host your web app there on a local IP that is visible only to one's system by connecting to another host computer or web server like Apache.



## Ping

* Facts
  * Determine the reachability of a specific destination
  * Determine the IP resolved from a specific hostname
  * Displays the latency of the network connection

* Challenges
  * What is the IP address of codepath.com?
      * The IP address if **codepath.com** is 198.58.125.217.
  * What is the IP address of google.com?
      * When I ping, it shows 64.233.185.100 but it changes.
  * Why would the IP address of google.com change between runs or from different locations?
      * Since Google is highly available, it has multiple instances of its same application running on thousands of machines. Thus, it makes these servers available for a large number of simultaneous requests. So, when my browser made a DNS request for getting its IP, it didn't receive a single IP but received many IP addresses that can serve the same website. Then, my browser chose one.  

## nslookup

* Facts
  * Command line interface for DNS server queries
  * Determine IP from hostname or hosts from IP
  * Often (but not always) helpful for determining an IP's provenance

* Challenges
  * Using the IP for codepath.com from the previous, pass it to nslookup
  * Does the domain returned from nslookup match? If not, why not?
      * The returned output is: <br>
      Non-authoritative answer:
          * 217.125.58.198.in-addr.arpa  
          * **name = thecodepath.com**
  and it does not match the result which was returned from **ping codepath.com**. Instead, it gives us the ip address of DNS server that is responsible for ip address we looked up. Tutorial Video: https://www.youtube.com/watch?v=jf-x76XYY2o

## traceroute

  * Facts
      * Determine the path to a specific destination
      * Each step in the path is called a hop
      * Determine the latency at each hop

  * Challenges
      * Compare the traceroutes for codepath.com and google.com
          * **traceroute codepath.com** made 12 hops while **traceroute google.com** made 25 hops. The first 11 hops in the terminal outputs are the same. This is because, the hops are going through my network to my ISP. Hops from my network to my ISP are the same. Then, my ISP will hop to google's server which is different from codepath.
      * How many of the hops are the same? What accounts for this?
          * 11 hops are the same because it is hopping from my network all the way to my Internet Service Provider.
      * Which has more hops? What accounts for the difference?
          * Google has more hops because it has multiple addresses running the same instance of an application and my request needs to go through load balancers.

## telnet
* Facts
  * Both a protocol and a command line utility
  * Part of the early internet
  * Largely abandoned in favor of ssh
  * Still useful to determine if a port is open
* Challenges
  * What's one thing that makes telnet insecure?
      * Telnet(Telecommunication Network)client is used to connect to device remotely over the network. All of the communication between your workstation and that telnet service is unencrypted including your username and password used to log onto that remote device.
  * Can you telnet to codepath.com? What port is open and why?
      * I tried telnetting codepath but wasn't able to find a port that allows telnet connection.


# Core Tools

  * The following are very widely used by engineers of all stripes and will be invaluable over the course of a career. If you don't know them, you need to. If you're going to spend extra time with any of the tools in this milestone, these are the ones to know.

## curl and wget

* Facts
    * Both can download contents from FTP, HTTP and HTTPS
    * Both can send HTTP POST requests and use HTTP cookies
    * Both are scriptable

* Challenges
    * Identify some differences between the two
    * Which would you be more likely to use for interacting with a RESTful API from the command line?
    * Which support recursive downloading?
    * Which are you more likely to find pre-installed on a Linux OS?
    * What is the syntax for each for downloading a file to the current directory?
