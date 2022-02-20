High level approach:

First, when a bridge starts up, it will believe that it is the root and it has cost of 0. When it is set, it will broadcast every port about its bpdu.
Then when a message is recieved by any port in this bridge, we first check that if it is a 'bpdu' type of message or a normal message send by host.  If this is a message type of bpdu, we needs to updating our current root.
In order to find better bpdu, we first compare for lower bridge id, if it is the same, then we compare for lower cost, and if it is also the same, we then compare for lower bridge id.
In this way, we are able to store the root bridge and acknowledged by every bridge in each LAN. Then we check if every bridge has form the same idea that the root is the same.
If it is, then our spanning tree is ready and then we are constructing the bridge's designate port and block port.
The desginate port are the ports that it compares with all the bpdu messages that this port heard from this LAN. Then we compare to find the best
by first comparing the cost to root bridge and then the id. Meanwhile, the block other port that are not root or designate port.

After this, when a message come, if it comes from the blocked port, we ignore it. If it comes from designate or root port, we check to see if it satisfies our forwarding table
and records. If there is a destination match, we then send it to the port that connect to its destination. If not, then we broadcast to every port that are not blocked.

Then if a new bridge starts up, then it will rebroadcast its bpdu to others and trigure them to check and configure that new bridge into the spanning tree.

If a bridge dies, then the previous bpdu message that a bridge will constantly send (every 500 ms) will stopped and it will be captured by other bridge
that check every 750 ms. If the previous bridge stopped, then we decide whether it is the root port direction that this bridge stopped or designate port direction.
If it is the root port, then we need to reconfigure the entire bridge such that maintaining the spanning tree. If it is not the root that stopped, then we only need
to reconfigure the port that it heard stopped.

Challenge:

The first challenge is in mile stone 2 where we are trying to figure out why their are loops inside our spanning tree. It was between the methods of determining root bridge and checking whether 
the spanning tree has formed. In these two methods, we tried in debugging by printing out every detail of our construction and trying to find out the problem.

The second is when we are constructing the method for handling stop and start of bridges. At this point, we are keep trying to increase our goodput as well as
maintaing a certain amount of package delivery. The output, seems like difference for every time with difference input and difference starting sequence.

Features:
Our feature is that we have stored a special table called PORT_LIST that keep track of the port and messages that come from the port every time.  By doing this, we are able to achieve information
using dictionary in python and storing more information. 


Testing:
We have tested our code by printing and flushing out output in different stage of our bridge. There are clear message printed on the screen when the bridge is starting up and communicating with the bpdu.
It will prints out the change each time it finds a better bpdu and changing the root port. Meanwhile, when finishing with the configuration, it will print the final output about its designated port and blocked port.

Then when recieving message, it will print out the message about whether it will broadcast, forward to certain port, or ignoring.