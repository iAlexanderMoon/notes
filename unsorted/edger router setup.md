EdgeRoute 

show


port-forward {
    auto-firewall enable
    hairpin-nat enable
    lan-interface switch0
    rule 1 {
        description Wireguard
        forward-to {
            address 192.168.1.99
            port 51820
        }
        original-port 51820
        protocol udp
    }
    rule 2 {
        description SSH
        forward-to {
            address 192.168.1.99
            port 22
        }
        original-port 51822
        protocol tcp_udp
    }
    wan-interface eth0
}


```
configure

set port-forward rule 3 description https
set port-forward rule 3 forward-to address 192.168.1.91
set port-forward rule 3 forward-to port 443
set port-forward rule 3 original-port 443
set port-forward rule 3 protocol tcp

set port-forward rule 4 description http 
set port-forward rule 4 forward-to address 192.168.1.91
set port-forward rule 4 forward-to port 80
set port-forward rule 4 original-port 80
set port-forward rule 4 protocol tcp

set port-forward rule 5 description http8000
set port-forward rule 5 forward-to address 192.168.1.91
set port-forward rule 5 forward-to port 8000
set port-forward rule 5 original-port 8000
set port-forward rule 5 protocol tcp

set port-forward rule 6 description https9000
set port-forward rule 6 forward-to address 192.168.1.91
set port-forward rule 6 forward-to port 9000
set port-forward rule 6 original-port 9000
set port-forward rule 6 protocol tcp

commit ; save


Wow that works! So... can we host drone at a different domain or default path?
behind the proxy?

# [[acme.domains]]
#   main = "local1.com"
#   sans = ["test1.local1.com", "test2.local1.com"]

