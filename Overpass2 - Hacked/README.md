# Writeup for TryHackMe Room '[Overpass 2 - Hacked](https://tryhackme.com/room/overpass2hacked)'
## 'Overpass 2' is a great beginner walkthrough room containing concepts from Research , Analysis and Forensics. 

## [Task 1] Forensics - Analyse the PCAP
** We are given a PCAP file ``overpass2.pcapng`` by hints of which looks like we need to investigate packets of data using wireshark , so we fire up wireshark **

``wireshark overpass2.pcapng``

# ![2](Images/wireshark_get_req.png?raw=true "scan")

** At first glance itself of the packet we can observe GET requests and some HTTP requests. Let's observe them in detail so that we can understand them better. **

# ![3](Images/get.png?raw=true "scan")

** This GET request doesn't seem to have any valuable information , lets go on to the next HTTP request. **