# AWS Direct Connect

## Getting Started

### Step 1: Create a Connection
```
$ aws directconnect describe-locations

$ aws directconnect create-connection \
--location TIVIT \
--bandwidth 1Gbps \
--connectionname "Connection to AWS"
```

### Step 2: Download the LOA-CFA
```
$ aws directconnect describe-loa \
--connection-id dxcon-fg31dyv6 \
--output text \
--query loaContent|base64 --decode > myLoaCfa.pdf
```

### Step 3: Create a Virtual Interface and get the Router Configuration
```
$ aws ec2 describe-vpn-gateways

$ aws directconnect create-private-virtual-interface \
--connection-id dxcon-fg31dyv6 \
--new-private-virtual-interface virtualInterfaceName=PrivateVirtualInterface,vlan=101,asn=65000,virtualGatewayId=vgwebaa27db,addressFamily=ipv4

$ aws directconnect describe-virtual-interfaces \
--virtual-interface-id dxvif-ffhhk74f \
--query virtualInterfaces[*].customerRouterConfig \
--output text
```

- Sample Route Configuration (CISCO IOS)
```
interface GigabitEthernet0/1
no ip address

interface GigabitEthernet0/1.VLAN_NUMBER
description "Direct Connect to your Amazon VPC or AWS Cloud"
encapsulation dot1Q VLAN_NUMBER
ip address YOUR_PEER_IP

router bgp CUSTOMER_BGP_ASN
neighbor AWS_PEER_IP remote-as AWS_ASN
neighbor AWS_PEER_IP password MD5_key
network 0.0.0.0 
exit

! Optionally configure Bidirectional Forwarding Detection (BFD).

interface GigabitEthernet0/1.VLAN_NUMBER 
bfd interval 300 min_rx 300 multiplier 3 
router bgp CUSTOMER_BGP_ASN 
neighbor AWS_PEER_IP fall-over bfd 

! NAT Configuration for Public Virtual Interfaces (Optional)

ip access-list standard NAT-ACL
permit (internal subnet IP address for NAT)
exit

ip nat inside source list NAT-ACL interface GigabitEthernet0/1.VLAN_NUMBER overload

interface GigabitEthernet0/1.VLAN_NUMBER
 ip nat outside
exit

interface interface-towards-customer-local-network
 ip nat inside
exit
```

### Step 4: Verify Your Virtual Interface using traceroute

## Locations
- describe-locations

## Create a Connection
- create-connection
- confirm-connection
- delete-connection
- describe-connections

## Hosted Connection
- allocate-hosted-connection
- associate-hosted-connection
- describe-hosted-connections

## Loa
- describe-loa

## Virtual Interfaces

### General
- associate-virtual-interface
- delete-virtual-interface
- describe-virtual-interfaces
- update-virtual-interface-attributes

### Public
- allocate-public-virtual-interface
- confirm-public-virtual-interface
- create-public-virtual-interface

### Private
- allocate-private-virtual-interface
- confirm-private-virtual-interface
- create-private-virtual-interface

### Transit
- allocate-transit-virtual-interface
- confirm-transit-virtual-interface
- create-transit-virtual-interface

## BGP Peer
- create-bgp-peer
- delete-bgp-peer

## LAGs
- create-lag
- delete-lag
- describe-lags
- update-lag
- associate-connection-with-lag
- disassociate-connection-from-lag

## Direct Connect Gateways
- create-direct-connect-gateway
- delete-direct-connect-gateway
- describe-direct-connect-gateways
- describe-direct-connect-gateway-attachments

## Direcrt Connect Gateway Proposals
- accept-direct-connect-gateway-association-proposal
- create-direct-connect-gateway-association-proposal
- delete-direct-connect-gateway-association-proposal
- describe-direct-connect-gateway-association-proposals

## Direct Connect Gateways Associations
- create-direct-connect-gateway-association
- delete-direct-connect-gateway-association
- describe-direct-connect-gateway-associations
- update-direct-connect-gateway-association

## Tag
- tag-resource
- untag-resource
- describe-tags

## Interconnect
- create-interconnect
- delete-interconnect
- describe-interconnects

## Virtual Gateways
- describe-virtual-gateways
