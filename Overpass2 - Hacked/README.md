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

### #1  What was the URL of the page they used to upload a reverse shell?
This gives us the answer to our first question. This GET request doesn't seem to have any other valuable information , lets go on to the next HTTP request.

# ![4](Images/HTTP.png?raw=true "scan") 
Here we can see the payload that the attacker used to compromise the system.

### #2 What payload did the attacker use to gain access?
Do include the php tags along with the payload.

The attacker managed to start a reverse shell on the system (/development/uploads/payload.php GET request) beyond which all the instructions sent over the network are in plaintext and can be observed below.

### #3 What password did the attacker use to privesc?

# ![4](Images/JamesPassword.png?raw=true"Scan")

### #4 How did the attacker establish persistence?

The attacker downloaded a backdoor on the system 

# ![5](Images/Persistence.png?raw=true"Scan")

### #5 Using the fasttrack wordlist, how many of the system passwords were crackable?

Just select and copy the contents of /etc/shadow visible in the TCP Stream and save in a file and run John The Ripper on it with the wordlist Rockyou.txt , you'll get the answer.

## [Task 2] Research - Analyse the code 
Now that you've found the code for the backdoor, it's time to analyse it.

Go ahead and download the backdoor.

``git clone https://github.com/NinjaJc01/ssh-backdoor``

The first thing that should come to our mind would be to examine the main function so that we could have the idea of what is going on in the backdoor and what functions it is using and how it works.

Examine the main function ``main.go``

### #1  What's the default hash for the backdoor?

# ![6](Images/defaulthash.png?raw=true"Scan")

The variable hash string comprises of the default hash for the backdoor.

### #2  What's the hardcoded salt for the backdoor?
One of the worst mistakes a developer commits, is to use hardcoded values, it makes exploitation 10 folds easier.

# ![7](Images/hardcodedsalt.png?raw=true"Scan")

### #3  What was the hash that the attacker used? - go back to the PCAP for this!

# ![8](Images/Attackerhash.png?raw=true"Scan")

### #4  Crack the hash using rockyou and a cracking tool of your choice. What's the password?

Take the above hash and use Hashcat or John The Ripper to crack it, as it is salted online hash cracking tools won't work.

``hashcat -m 1710 -a 0 hash:salt /usr/share/wordlists/rockyou.txt``

## [Task 3] Attack - Get back in! 

Now that the incident is investigated, Paradox needs someone to take control of the Overpass production server again.

There's flags on the box that Overpass can't afford to lose by formatting the server!

### #1  The attacker defaced the website. What message did they leave as a heading?
Deploy the machine and go to the IP Address provided and you'll be able to see the message crystal clear.
### #2 What's the user flag?

# ![9] (Images/Backdoor_Port.png?raw=true"Scan")
As we know that the backdoor was setup on port 2222 , we can expect it to run a ssh service on the same. To login we will use the previously gained password which we cracked and the username of james. 

### #3 What's the root flag?
