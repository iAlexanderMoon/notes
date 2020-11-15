# Home Network Readme for Self Hosting:

## TLDR; Abstract

Self hosting WAN Services and LAN services on seperate servers and networks using debian, docker, and docker compose.

## Instructions


## Background

### Servers

Whether physical or virtualized a straight forward setup will run the WAN server in a seperate network from  

## Wide Area Network (WAN) Services

Services run which are exposed to the greater internet.

### Finding your WAN from the greater internet

Your ISP is most likely issuing a dynamic IP address to your house. Even though this is dynamic it's not likely changing that often. There are a couple options beyond the scope of this document for updating a DNS entry your your own domain, or a shared dynamic domain such that you can use that address to get to your ip. Having your own domain and updating DNS entry control can be important depending on the types of things you are hosting that are facing the internet Wide Area Network (WAN). 

## LOCAL AREA NETWORK (LAN) Services

Services which are available to devices connected to your home network.

Networks can be further partitioned/segregated into smaller networks for more security (for example seperating all your IOT devices into a seperate network, or maybe security cameras etc) these are topics for the advanced self hoster.

### Finding Local Services

Whether it's your smart phone, computers, printers, other devices it's nice to be able to find the services/devices you are looking for. multicast dns has finally been implemented in windows 10. For linux (Avahi) and apple (bonjour) and services publishing to the network using zeroconf mdns.

#### Smartphones mdns lookup

Most smartphones and now browsers are using direct to dns lookup without a lookup for mdns .local addresses. 

