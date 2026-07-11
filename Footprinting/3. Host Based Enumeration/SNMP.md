**Basics**
- Simple Network Management Protocol
- Monitor network devices
- Can configure devices remotely
- Configuration file: /etc/snmp/snmpd.conf

**Hardware**
- Routers
- Switches
- Servers
- IoT devices etc.

**Ports**
- Commands via UDP 161

**Traps**
- UDP port 162
- These are data packets sent to client without being requested
- Can be configured to send packets in any pattern

**MIB**
- Management Information Base
- An MiB is a text file in which all queryable SNMP objects of a device are listed in a tree
- Independent format for data storage
- Contains at least one **Object Identifier (OID)**
- **Object Identifier (OID)**
	- Provides information about type, access rights, and a description of the respective object
- Written in **Abstract Syntax Notation One (ASN.1)** based ASCII format
- Does not contain data itself, but explain where to find the information and what the information is like

**OID**
- Represents a node in the hierarchical namespace
- Has an identification number
- Sometimes contain only reference to another node below them

**Community Strings**
- Passwords used to determine visibility of the requested information

***Different Versions of SNMP***
1. **SNMPv1**
	- Supports data retrieval, device configuring and traps
	- No built-in authentication
	- Does not support encryption
2. **SNMPv2**
	- Currently v2c is available (c for community-based)
	- Faster than SNMPv1
	- Supports GETBULK command
	- Improved version of SNMPv1
	- Same security as SNMPv1
3. **SNMPv3**
	- Provides authentication
	- Encryption via **pre-shared key**
	- Extremely complex

**Dangerous Settings**
- `rwuser noauth`: Provides full access to the full OID tree without authentication
- `rwcommunity <community string> <IPv4 address>`: Provide access to the full OID tree regardless of where the requests are sent from
- `rwcommunityv6 <community string> <IPv6 address>`: IPv6 version of previous setting

**Footprinting the Service**
- Following tools can be used:
	1. snmpwalk
		- Used to query the OIDs with their information
	2. onesixtyone
		- Used to brute-force the names of community strings
	3. braa
		- Used to query multiple OIDs across many hosts simultaneously

1. **SNMPwalk**
	- snmpwalk -v2c -c public [ip address]
2. **OneSixtyOne**
	- onesixtyone -c [seclists location] [ip address]
3. **Braa**
	- braa <community string>@[ip address]:.1.3.6.*