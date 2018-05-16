### OpenVPN setting

1. install
```
$ sudo apt-get install openvpn libssl-dev openssl easy-rsa
```
1. firewall setting
  -  Not working(default)
1. download .opvn file
  - VPN Gate client setting file
http://www.vpngate.net/ja/do_openvpn.aspx?fqdn=vpn427216179.opengw.net&ip=106.166.229.58&tcp=1916&udp=1832&sid=1524378890468&hid=2219713
1. connection@Terminal
  - config path : /etc/openvpn/config
```
$ sudo openvpn --config vpngate_vpn427216179.opengw.net_udp_1832.ovpn
```

---

- CA(Certification Authority)
  - ca.crt
- Client Authority
  - username.crt
- cleint private key
  - username.key
