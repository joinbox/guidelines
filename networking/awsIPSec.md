# Connect to a VPC with pfsense 2.1 via IPSec using BGP

### Example Setup

- AWS Subnet: 10.100.0.0/16
- Local Subnet: 10.0.0.0/16
- Local public IP: 9.9.9.9
- Dynamic Rounting via BGP
- Redundant connections to your VPC


## Install pfsense

Install [pfsense](https://www.pfsense.org/) version > 2.1 on any host with at least two network interfaces. The WAN interface must have a public IP reachable directly by the amazon vpc infrastructure. the other interface is in your local private subnet.



