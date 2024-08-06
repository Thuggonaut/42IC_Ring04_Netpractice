# Net_Practice
- System Administration and Networking Practice
- Solve network problems 

### Note:
Each level has personal notes and questions I needed answered specific to the level to help me progress. 

# Review:

### Configure small-scale networks

🧐 ***How does TCP/IP addressing work?***
- TCP/IP uses these components to enable communication between devices across networks and the internet:

	1. **IP addresses:** Unique identifiers for devices on a network (e.g., 192.168.1.1).
		- IPv4 (32-bit) and IPv6 (128-bit) are the two main versions.
		- IPv4 example: 192.168.1.1
		- IPv6 example: 2001:0db8:85a3:0000:0000:8a2e:0370:7334

	2. **Subnets:** Divide networks into smaller segments (e.g., 255.255.255.0).
		- Subnetting allows for more efficient use of IP addresses and simplifies network management.
		- Subnet masks determine which part of an IP address belongs to the network and which part identifies the host.
		- Common subnet masks:
			- 255.0.0.0 (/8)
			- 255.255.0.0 (/16)
			- 255.255.255.0 (/24)

	3. **Ports:** Identify specific services on a device (e.g., 80 for HTTP).
		- Ports are used to distinguish between different services running on the same host.
		- Well-known ports (0-1023), registered ports (1024-49151), and dynamic/private ports (49152-65535)
		
	4. **DNS:** Translates domain names to IP addresses.
		- DNS servers store mappings between domain names and IP addresses.
		- DNS resolution involves querying DNS servers to find the IP address associated with a domain name.

	5. **DHCP:** Automatically assigns IP addresses to devices.
		- DHCP servers allocate IP addresses to devices on a network.
		- Uses a client-server model.
		- Assigns IP addresses, subnet masks, default gateways, and DNS servers.

	6. **Routing:** Directs data between networks using IP addresses.
		- Routers use routing tables to determine the best path for data to travel.
		- Protocols like RIP, OSPF, and BGP help routers exchange information.

	7. **NAT:** Translates private IP addresses to public ones for internet access.
		- Provides an additional layer of security by hiding internal network structure.
		- Allows multiple devices to share a single public IP address. 

	8. **Firewalls:** Protects networks from unauthorized access.
		- Network security devices that monitor and control incoming/outgoing traffic based on security rules.
		- Types: Packet filtering, stateful inspection, and application-level gateways.

