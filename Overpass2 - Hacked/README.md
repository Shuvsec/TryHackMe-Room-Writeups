# Writeup for TryHackMe Room '[Overpass 2 - Hacked](https://tryhackme.com/room/overpass2hacked)'
## 'Overpass 2' is a great beginner walkthrough room containing concepts from Research , Analysis and Forensics. 

## [Task 1] Forensics - Analyse the PCAP
Overpass has been hacked! The SOC team (Paradox, congratulations on the promotion) noticed suspicious activity on a late night shift while looking at shibes, and managed to capture packets as the attack happened.

Can you work out how the attacker got in, and hack your way back into Overpass' production server?

Note: Although this room is a walkthrough, it expects familiarity with tools and Linux. I recommend learning basic Wireshark and completing [CC: Pentesting](https://tryhackme.com/room/ccpentesting) and [Learn Linux](https://tryhackme.com/room/zthlinux) as a bare minimum.

We are given a PCAP file ``overpass2.pcapng`` by hints of which looks like we need to investigate packets of data using wireshark , so we fire up wireshark.

``wireshark overpass2.pcapng``

# ![2](Images/wireshark_get_req.png?raw=true "scan")

At first glance itself of the packet we can observe GET requests and some HTTP requests. Let's observe them in detail so that we can understand them better.

# ![3](Images/get.png?raw=true "scan")

This GET request doesn't seem to have any valuable information , lets go on to the next HTTP request.

# ![4](Images/HTTP.png?raw=true "scan") 