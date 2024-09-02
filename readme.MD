# Net_Practice
- System Administration and Networking Practice
- Solve network problems 

### Note:
Each level has personal notes and questions I needed answered specific to the level to help me progress.
Check out my guide on YT [here](https://youtu.be/HQUw0CfQWAM) 

# Review:

### Configure small-scale networks

🧐 ***How does TCP/IP addressing work?***
- TCP (Transmission Control Protocol) is a communication protocol used on the internet to ensure that data is sent and received accurately between devices. 
- These list of rules/standards are divided into layers:
	- Application L7: e.g. Web browsers
	- Transport: Ports
	- Network L3: IP addressesm routers
	- Physical (Physical L1 and Datalink L2): Ethernet cables

🧐 ***What about the OSI model?***
- Like TCP/IP, has the same layers, but with some extras. 
- Application has layers:
	- Presentation
	- Session
	- These are actually inside the application layer of TCP/IP 

# LAN:
- Is a Local Area Network.
- It has a network of devices talking to each other within a limited range (e.g. your home network, with multiple devices connected with each other, at the same time talking to the internet). 

## IP:
- Internet Protocol
- A logical address of a device in the network (provided by a router).
- This allows for identification and location of a machine/device on a LAN.
- More importantly, allows for devices to talk to each other, like how mobile phones have phone numbers.
- Each device will have a unique IP (e.g., `192.168.1.1`). 
	- IPv4 (32-bit) and IPv6 (128-bit) are the two main versions.
	- IPv4 example: `192.168.1.1` four octets separated by dots. The first 3 is the `Network Portion` the 4th is the `Host`
	- IPv6 example: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- ***Usable IPs to assign to devices/hosts***
	- There will always be two IPs reserved and unusable: 
		- the Network Address (e.g. `192.168.1.0`) 
		- the Broadcast Address (e.g. `192.168.1.255`)
	- If there is a Default Gateway, it's address may be `192.168.1.1`
	- Usuable IPs will then begin at `192.168.1.2` and end at `192.168.1.254`

## SUBNET:
- Identifies whether a machine/device belongs to a LAN, or a network external from the LAN. 
- A subnet is a logical subdivision of an IP network, allowing for better management and organization.
- A set of flags which defines the overall range of a LAN. 
- For example:
	- A laptop's IP is
	`198.168.10.20`
	- The printer's IP is 
	`198.168.1.2` 
	- The subnet mask that defines the range for these devices may be
	`255.255.0.0`
	- The `255`s in the subnet mask indicate which part of the IP address is the network portion (fixed) and which part is the host portion (variable).
	- This means any device that falls within the `192.168.x.x` range is potentially a device in the LAN.
	- The last two octets of the IPs can vary between `0` and `255` within the subnet range defined by the subnet mask.
- Common subnet masks:
	- 255.0.0.0 (/8)
	- 255.255.0.0 (/16)
	- 255.255.255.0 (/24)
- ***Converting the subnet mask to bits***
- e.g. `255.255.255.0` 
	- converted into binary is `11111111.11111111.11111111.00000000` (1s and 0s will always be contiguous).
	- All the 1s tell us they are the network bits, and wouldn't change. 
	- All the 0s tell us which of the IP are the host bits. This means each of these bits can be used to create an IP on the network, to assign to a host. 
		- The 0s also tell us how many hosts can be in a network (2^0s) e.g., 2^8 gives is 256, - 2 (reserved IPs) = 254 availble IPs to assign to hosts. 
		- If a network requires more host IPs, then we take 1 of the network bits and convert it to a 0, e.g. `111111111.11111111.11111110.00000000` giving us 2^9 = 510 usable IPs (we just SUBNET'ED). 
			- Which will make it `255.255.254.0` in decimal, our new subnet mask.
- ***Subnetting to create more networks***
- e.g. our network: `192.168.1.0/24` (/24 is the subnet mask), `255.255.255.0` is the subnet mask. We need to create 4 networks:
	- by hacking the Host bits (0s), flipping them to 1s
	- More networks, requires more bits
	- How many bits do we need to create 4 networks?
		- Recall, to convert decimal to binary, the power of 2s: `128 64 32 16   8 4 2 1`
		- 2 x that and we have `256 128 64 32   16 8 4 2`
		- We need 4 networks, which will require 2 bits from the host bits:
		```c
		 -   -  -  -     - - I I
		256 128 64 32   16 8 4 2
		```
			- Our subnet mask was `11111111.11111111.11111111.00000000`. 
			- Hacking 2 bits will give us `11111111.11111111.11111111.11000000`
			- Our new subnet mask is now `255.255.255.192` in decimal.
			- Giving us a `/26` CIDR notation (26 network bits total out of 32).
		- Our network address is `192.168.1.0`. What is our broadcast address for this network?
			- We need to find the increment (the last network bit)
				- Recall, our new subnet mask in binary is `11111111.11111111.11111111.11000000`
				- The last bit is `64`
				- `64` determines the size of our network and range (`192.168.1.0 - 192.168.1.63`). 
		- We now have 4 networks:
			- `192.168.1.0 - 192.168.1.63`
			- `192.168.1.64 - 192.168.1.127`
			- `192.168.1.128- 192.168.1.191`
			- `192.168.1.192 - 192.168.1.255`
			- Each will have the subnet mask of `255.255.255.192` OR `/26`.
		- But we also now have less host bits (six 0s) which gives us 64 - 2, available IPs for hosts in a network. 
- ***Reverse Subnetting***
- Solving problems (like in NetPractice)
- What is the Network Address; Broadcast Address; and Range of a network configuration?
- Steps:
	1. Convert a subnet mask from decimal to binary.
	2. Find the CIDR notation (how many network bits).
	3. Find the increment (the last network bit of the subnet mask).
	4. Create the networks.
		1. Start with the IP address and its mask in binary.
			- e.g. for IP: `172.17.16.255`
			- the mask: `11111111.11111111.11110000.00000000`
			- `/20`
		2. Find the network ranges. Recall, all the network bits of the mask is fixed. 
			- The increment is in the 3rd octet `16`
			- So the ranges are:
				- `172.17.0.0 - 172.17.15.255`
				- `172.17.16.0 - 172.17.31.255` (our IP is here)
				- `172.17.32.0 - ... etc`
		3. Check if our IP is valid. Should it be in the same network it's trying to communicate with, but isn't? Is it in the same network as its router? 

## SWITCH:
-  A device that sits within a LAN, and connects multiple devices within the LAN to help them talk to each other. 
- For example, say, a laptop sends a request to print a file. The switch will identify the request needs to go to the printer's IP. 
- A switch can also be connected to a router. 

## ROUTER:
- A networking device that sits between the LAN and the external network (e.g. internet). 
- It connects us to the internet, and other networks.
- Routers typically have multiple IP addresses for different networks. 
- It forwards data packets between **different** computer networks. 
- For example, a mobile phone wants to connect to the internet, the mobile sends a request to the router, then the router connects to the internet.
- Routers use routing tables to determine the best path for data to travel.
- Routers are layer 3 devices (Network layer of the OSI (Open Systems Interconnection)) and operate at the network layer, handling the routing of data packets based on IP addresses.
- Layer 3 sends data packets intructions for the router, while layer 2 (Data Link Layer) sends instructions for the switch. Layer 1 (Physical Layer) is the physical ports.
- A router will have a group of networks, e.g. `10.1.1.0/24` which means this group of network has IP addresses from `10.1.1.0 - 10.1.1.225`.
- The **next-hop IP** is the address of the next router or device that will receive the packet.

## GATEWAY: 
- A gateway usually has a single IP address (of the router) that serves as the entry/exit point for a network.
- A gateway and router is often the same device, the only difference is their function:
	- Router: Primarily routes data packets between different networks. It determines the best path for data to travel based on IP addresses.
	- Gateway: Serves as a "gate" between two networks, allowing them to communicate. It can also translate different protocols or data formats, connecting networks that use different methods of communication, such as:
		- TCP/IP to IPX/SPX
		- HTTP to FTP
		- Ethernet to Wi-Fi
- A **point-to-point link** is a direct connection between two network devices, allowing them to communicate exclusively with each other:
	- It connects two endpoints directly, such as two routers or a router and a switch.
	- A default gateway sends data from one network to another.
	- Often uses a /30 subnet mask, which allows for only two usable IP addresses (one for each device).



# WAN:
- Wide Area Network
- A network that covers a broad area, often connecting multiple LANs (Local Area Networks) across cities, countries, or even globally.

## ROUTER: 
- A router must have 4 funcitonalities to deliver as a gateway device:
	- **NAT:** 
		- Network Address Translation, translates private IP addresses within a local area network (LAN) to a public IP address for internet access. 
		- When a LAN device sends a request to the router, the IP of said device will not be exposed to the WAN. 
		- Allows multiple devices to share a single public IP address. 
		- Typically, a LAN device sends a request to the router `=>` the router requests data from a WAN device `=>` the data gets sent back to the LAN device.
	- **FIREWALL:** 
		- Set of passive rules to protect the network from unauthorized access.
		- These define standards of communication between internal and external networks. 
		- To allow communications through the firewall, we configure:
			1. DMZ
			2. Port forwarding
	- **DMZ:**  
		- Demilitarized Zone, created within the LAN, and specifies to the router, that whenever an WAN external ad-hoc network wants to establish a connection with the LAN, the router passes the request through the DMZ. 
		- If there is a device sitting in the DMZ, that device will communicate directly with the WAN device outside the firewall. 
	- **PORT FORWARDING:** 
		- Similarly to DMZ, sets standards for communication between the WAN and the LAN outside the firewall.
		- But it does not create a seperate DMZ. Rather, it specifies on which ports, communbications on certain LAN devices need to occur. 
		- A port is a virtual point where network connections start and end, allowing different machines to communicate through specific channels.
		- For example:
		HTTP uses port 80 (web browsing)
		HTTPS uses port 443 (secure web browsing)
		FTP uses port 21 (file transfew between computers over the internet).
		- Say, a WAN device wants to communicate with a LAN device. It will only be approved if its on port 80 as configured. 
		 



# References:
- Networking basics: https://www.youtube.com/watch?v=_IOZ8_cPgu8
- You Suck at Subnetting, Playlist by Network Chuck: https://www.youtube.com/playlist?list=PLIhvC56v63IKrRHh3gvZZBAGvsvOhwrRF
- IP Subnet Calculator: https://www.calculator.net/ip-subnet-calculator.html
