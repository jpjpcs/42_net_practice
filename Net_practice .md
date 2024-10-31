# Introduction

Net_Practice project is a general practical exercise about **==networking==**.
We have to configure small-scale networks, which means we need to understand how **==TCP/IP adressing==**. 

The project is divided into 10 levels.
[[Net_practice lvl.1|Level 1]]
[[Net_practice lvl.2|Level 2]]
[[Net_practice lvl.3|Level 3]]
[[Net_practice lvl.4|Level 4]]
[[Net_practice lvl.5|Level 5]]
[[Net_practice lvl.6|Level 6]]
[[Net_practice lvl.7|Level 7]]
[[Net_practice lvl.8|Level 8]]
[[Net_practice lvl.9|Level 9]]
[[Net_practice lvl.10|Level 10]]

# Key Concepts

## Masks
---
A mask is something like this: **==255.255.255.0==**
The mask is basically what keeps our network organised.

#### What does it mean to have a mask 255.255.255.0?
---
Having a network where our mask is 255.255.255.0 will mean that our network will need to obey to some rules.
One of them being, wherever in the mask is 255 the IPs must mach that!
Like so:
Mask : 255.255.255.0
IP 1 : 183.235.234.133 - Valid IP
IP 2 : 183.235.234.123 - Valid IP
IP 3 : 183.235.123.102 - Invalid IP

IP 3 is invalid in our example network that has the mask 255.255.255.0 because the third set of bytes(183.235.==123==.102) isn't the same as IP 1 and 2(183.235.==234==.133).

Another example:
Mask : 255.255.0.0
IP 1 : 183.235.234.133 - Valid IP
IP 2 : 183.235.234.123 - Valid IP
IP 3 : 183.235.123.102 - Valid IP

In this case IP 3 is valid in our network because our mask this time only needs the first 2 sets of bytes to match(==183.235==.234.133)

### IP Range
---
Another thing that our mask sets is the IP Range i.e. the possible IPs.
Let's look at examples:

Mask     : 255.255.255.0/24

IP Range : 123.123.213.0 -> 123.123.213.255

#### What is the /24?
---
Within our mask there are rules that me must obey.
Contrary to many things in life, the appearance of a mask does not deceive one bit, literally...

##### What does that mean exactly?
---
Let's use the example of 255.255.255.0 again:

255      . 255      . 255      . 0        - This is our mask
11111111 . 11111111 . 11111111 . 00000000 - Also our mask

What the appearance of our mask is telling us is the following:
	- My name is **==255.255.255.0==**
	- You can also call me **==11111111.11111111.11111111.00000000==**
	- My CIDR(Class Inter-Domain Routing) is **==/24==**(8 + 8 + 8 + 0 = 24) 
	- That means you can have **==1 Subnet==**
	- With **==256 IP addresses==**
	- And **==254 Usable IP addresses==** (123.123.123.1 -> 123.123.123.254)
	- 123.123.123.0 and 123.123.123.255 are reserved
	- 123.123.123.0 reserved as **==Network Address==**
	- 123.123.123.255 reserved as **==Broadcast Address==**

Let me introduce you to my other mask friend, 255.255.255.128:

255      . 255      . 255      . 128
11111111 . 11111111 . 11111111 . 10000000

Now what does the appearance of my friend is telling you:
	- My name is **==255.255.255.128==**
	- You can also call me **==11111111.11111111.11111111.10000000==**
	- My CIDR(Class Inter-Domain Routing) is **==/25==**(8 + 8 + 8 + 1 = 25) 
	- That means you can have **==2 Subnets==** (1º from 0 to 128, 2º from 129 to 255)
	- With **==128 IP addresses==**
	- And **==126 Usable IP addresses per subnet==** (123.123.123.1 -> 123.123.123.126 and 123.123.123.13 -> 123.123.123.254)
	- Yes 123.123.123.0, 123.123.123.127, 123.123.123.128, 123.123.123.255 are reserved
	- 123.123.123.0 and 123.123.123.128 reserved as **==Network Address==**
	- 123.123.123.127 and 123.123.123.255 reserved as **==Broadcast Address==**

### Helping table
---
| CIDR | Dot-decimal | Number of IP-addresses<br /> per subnet | Usable IP-addresses <br /> per subnet | Number of subnets |
| :---: | :-----------: | :---: | :---: | :---: |
| /32 | 255.255.255.255 | 1 | 0 | 256 |
| /31 | 255.255.255.254 | 2 | 0 | 128 |
| /30 | 255.255.255.252 | 4 | 2 | 64 |
| /29 | 255.255.255.248 | 8 | 6 | 32 |
| /28 | 255.255.255.240 | 16 | 14 | 16 |
| /27 | 255.255.255.224 | 32 | 30 | 8 |
| /26 | 255.255.255.192 | 64 | 62 | 4 |
| /25 | 255.255.255.128 | 128 | 126 | 2 |
| /24 | 255.255.255.0 | 256 | 254 | 1 |

### Routing Tables
---
Routing Tables are essentially "maps" that direct the information to their respective paths.

They're usually composed by the following elements:
	- **Destination IP** : The IP address or network that the packet is meant to reach.
	- **Subnet Mask** : Defines the range of IP addresses within a network; it helps determine if an IP is part of a particular network.
	- **Next-Hop IP** : The IP address of the next device (router or gateway) along the path to the destination. If there’s no next hop, the destination is directly connected.
	- **Interface** : The network interface (e.g., Ethernet or Wi-Fi port) used to send the packet.
	- **Metric** : A value that represents the cost or priority of the route. Lower metrics are preferred routes.

For Net_Practice we'll only need knowledge of the first three points. Destination IP, Subnet Mask and Next-Hop IP.

#### Routing table example:

![[Pasted image 20241027162248.png]]

What we're saying in this routing table is that:
	- **Destination IP** is 192.168.0.0/26(Network address we want to send information to)
	- **Next-Hop** is 10.0.0.2(Destination of the next router you need to send information in order to reach the desired destination)