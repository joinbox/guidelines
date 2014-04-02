# Connect to a VPC with pfsense 2.1 via IPSec using BGP (two tunnels)

## Requirements: as public IP only used for that service.

## Example Setup

- AWS Subnet: 10.100.0.0/16
- Local Subnet: 10.0.0.0/16
- Local public IP: 9.9.9.9
- Dynamic Rounting via BGP
- Redundant connections to your VPC


## Install pfsense

Install [pfsense](https://www.pfsense.org/) version > 2.1 on any host with at least two network interfaces. The WAN interface must have a public IP reachable directly by the amazon vpc infrastructure. The other interface is in your local private subnet.


## Prepare your VPC

1. Navigate to the VPC Console
2. Create new «dynamic» Customer Gateway. Set the BGP ANS to «65000» and the IP Address to your public interface of your pfsense box (9.9.9.9)
3. Create a new Virtual Private Gateway and attach it to your VPC
4. Create a new VPN Connection using the newly created virtual private gateway and Customer Gateway. Select the «Dynamic» rounting option.
5. Download the VPN configuration. Select the «Generic» vendor.

## Set up Pfsense

### Install required Packages

1. Navigate to «System», «Packages»
2. Install the «OpenBGPD» package


### Configure Virtual IPs

1. Open the downloaded configuration file, it contains two sections. Each section is for one tunnel. 
2. Navigate to «Firewall», «Virtual IPs»
3. Add a new Virtual IP for each tunnel. The IP is specified in the config file under «Inside IP Addresses», «Customer Gateway» (somehting like «169.254.254.62/30»)


### Set up Routing

1. Navigate to «System», «Routing», «Gateways» Tab
2. Add a new Gateway for your WAN ip: Name: Gateway9999, Interface: WAN, Gateway: 9.9.9.9
3. Add another Gateway for your internal Network: Name: InternalNet, Interface: LAN, IP: 10.0.0.1 (should be the ip of the router of your local network)
4. Navigate to the «Routes» Tab
5. Add a route for each of the VPN tunnels. The Destination Network IP is specified in the config file under «Inside IP Addresses», «Customer Gateway» (somehting like «169.254.254.62/30»). Select The «Gateway9999» gateway (9.9.9.9)


### Set up BGP

1. Navigate to «Services», «OpenBGPD», «Settings» Tab
2. Set: Autonomous Systems (AS) Number: 65000, Holdtime: 30
3. Add the following Network: 10.0.0.0/16 (your local network)
4. Navigate to the «Groups» Tab
5. Create a new Group with the name «MyVPC» and the AS specified inthe config file (Virtual PrivateGateway ASN, same for both tunnels)
4. Navigate to the «Neighbors» Tab
5. Add a neighbor for each tunnel: The IP (Neighbor) is specified in the config file under «Inside IP Addresses», «Virtual Private Gateway» (somehting like «169.254.254.61»). Set the group to «MyVPC».

If you edit the raw config you must add a newline at the end of the file or else the bgp deamon will not be able to load the config.



### Configure IPsec
1. Navigate to «VPN», «IPsec», «Tunels» Tab
2. Add a new Tunnel (do this twice using the relevant part of the config file):
  - Interface: WAN
  - Remote gateway: The remote gateway IP is specified in the config file under «Outside IP Addresses», «Virtual Private Gateway» (somehting like «87.238.85.42»)
  - Authentication method: Mutual PSK
  - Negotiation mode: main
  - My identifier: My IP address
  - Peer identifier: Peer IP address
  - Pre-Shared Key: The Pre-Shared Key is specified in the config file under «Configure the IKE SA as follows», «Pre-Shared Key» (somehting like «fdg876asfbawe98r7nds9f76afnas0f»)
  - Policy Generation: Default
  - Proposal Checking: Default
  - Encryption algorithm: AES 128 Bits
  - Hash algorithm: SHA1
  - DH key group: 2 (1024 bit)
  - Lifetime: 28800
  - NAT Traversal: Enable






