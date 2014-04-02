# Connect to a VPC with pfsense 2.1 via IPSec using BGP

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

## Set up Pfsense





