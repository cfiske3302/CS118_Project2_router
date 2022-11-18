UCLA CS118 2022 Fall Quarter Project 2 (Simple Router)
====================================

The developers are supposed to test and run their code in a virtual environment that has all dependencies installed like a container or a virtual machine .
We have included a [`Vagrantfile`](Vagrantfile) and a [`Dockerfile`](Dockerfile) for the developers to build the virtual development environment,

## Makefile

The provided `Makefile` provides several targets, including to build `router` implementation.  The starter code includes only the framework to receive raw Ethernet frames and to send Ethernet frames to the desired interfaces.  Your job is to implement the routers logic.

Additionally, the `Makefile` a `clean` target, and `tarball` target to create the submission file as well.

You will need to modify the `Makefile` to add your userid for the `.tar.gz` turn-in at the top of the file.

## Academic Integrity Note

You are encouraged to host your code in private repositories on [GitHub](https://github.com/), [GitLab](https://gitlab.com), or other places.  At the same time, you are PROHIBITED to make your code for the class project public during the class or any time after the class.  If you do so, you will be violating academic honestly policy that you have signed, as well as the student code of conduct and be subject to serious sanctions.

## Known Limitations

When POX controller is restrated, the simpler router needs to be manually stopped and started again.

## Acknowledgement

This is inherited from UCLA CS118 2017 Spring Quarter Project 3 (http://web.cs.ucla.edu/classes/spring17/cs118/project-3.html).

## TODO

    ###########################################################
    ##                                                       ##
    ## REPLACE CONTENT OF THIS FILE WITH YOUR PROJECT REPORT ##
    ##                                                       ##
    ###########################################################
\n
naw


## Disucssion Notes
### processPacket:
packet reaches router, call processPacket
    prints packet size and Interface rn
- first, check it is being sent to the correct interface. It the MAC is not the same as the interface, drop, not for us
- Check if ARP or IP packet.
    - If request, look up in table and return MAC if you know the MAC address
        - We can technically ARP something not on our network. if this is the case, send a reply is MAC is in the table
    - If reply, add mapping to MAC table. 
        - itterate through packets in queue and send all packets waiting on this MAC address

- get IP packet
    - add packet to queue
    - sent ARP request, get reply for the new mapping. Add to ARP cache
    - on reply, remove all t

- IP packet
     - Verify Checksum
        - if issue, then drop the packet. this is not CRC
     - Match lenght in header to given length (if bad then drop)
     - IP version check (must be v4) (if not drop)
     - Perform ACL. verify should not be dropped
     - Check if destined for the router. if IP is one of router's, drop
     - Decriment the TTL of packet, it TTL=0, drop (max number of times it can travel, prevent loops)
     - Look up next hop IP. Look at routing table enteries (routing_table.hpp). 
        - dest -> final IP
        - GW -> nexthop IP
        - mask -> 
        - interface name -> which interface you have to sent it out on
     - Look up next hop IP in ARP cache
        - if not in there
            - queue/cache packet and send ARP request
        - else
            - forward as normal
    