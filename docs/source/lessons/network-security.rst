Network Security 
================

Network security is primarily concerned
with unauthorized access, denial, or, malicious modification of network
communication.  

ArpSpoof 
--------

The ARP Spoofing attack is a typical
“man-in-the-middle” attack by which a hacker who has gained access to
communication between two parties, can subsequently monitor and relay modified
communication between these parties without their knowledge. 

The ARP (Address Resolution Protocol) protocol is used to discover the hardware
MAC address given an IP address. An ARP message is broadcast to the local
network, with the IP in question and the corresponding machine responds with its
MAC address. The hardware MAC address is typically needed to route packets to
the right machine on a local network. The mapping between IP and MAC addresses
that have been discovered via ARP, are stored in an ARP table on each machine. A
hacker on the same LAN as two communicating machines can send an ARP message
providing their own MAC address in response to the IP address of either of the
two machines. Thus, in effect, the hacker “poisons” the ARP table of these
machines by injecting an erroneous IP address - MAC address mapping. As a
result, messages intended for these machines will be routed to the hacker,
allowing them to gain access to potentially sensitive data.  

**Demonstration**

The demonstration on CHEESEHub illustrates the ARP spoofing attack using three
separate machines; a victim, server, and, hacker. The “arpspoof” software
provided by the “dsniff” library is used in the hacker to perform an ARP
spoofing attack on the victim. The hacker provides its own MAC address in
response to the server’s IP address, causing all messages from the victim to the
server to be routed through the hacker. We verify the success of the attack in
two ways.  

* We inspect the ARP table on the victim and discover two separate MAC addresses for the server’s IP address, one each from the server and hacker.  
* We look at the routing of packets from the victim to the server (for a simple "ping" command) to discover that packets are delivered using two hops, first to the hacker and then to the server rather than a single hop directly to the server.


Step-by-step instructions for this demonstration are as follows:


Step 1: Add the ArpSpoof application

Step 2: View the added application

Step 3: Start the application’s containers

Step 4: Launch the individual containers (open in separate browser tabs)

Step 5: In the hacker page, open the Hacker.ipynb Jupyter notebook for
instructions


Step 6: In the server page, open the Server.ipynb Jupyter notebook (launches in
new tab)


Step 7: Launch a new terminal from the server Jupyter interface


Step 8: Determine the IP address of the server by typing “ifconfig” in this
terminal



Step 9: Launch a terminal in the victim page



Step 10: Find the IP address of the victim by typing “ifconfig” in the victim
terminal



Step 11: Look at the initial ARP table on the victim (it has only the gateway’s
entry)



Step 12: Ping the server’s IP 



Step 13: Check the ARP table on the victim again (it now has an entry for the
server’s IP)



Step 14: Also check the route to the server using “traceroute”



Step 15: Next, open a terminal on the hacker to perform the attack


Step 16: Run the “arpspoof” command with the victim and server IPs



Step 17: Ping the server from the victim again (wait for a bit to see this
output)



Step 18: Check the ARP table on the victim now (it has two entries with the same
MAC address but different IP addresses; also this MAC address is different than
what we observed for the server before; it is in-fact the hacker’s MAC address)



Step 19: Check the route from the victim to the server (there are now two steps;
first the hacker, then the server)



Step 20: Shutdown the application



**More Information**

For more information on ARP poisoning and man-in-the-middle
network attacks, see ?  
