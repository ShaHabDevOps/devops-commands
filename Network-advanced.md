# NETWORK-ADVANCED (Ubuntu)

## iptables / nftables / Routing / MTU / Drops

| Command                                              | Purpose                           |
| ---------------------------------------------------- | --------------------------------- |
| `iptables -L -n -v`                                  | List firewall rules with counters |
| `iptables -t nat -L -n -v`                           | Show NAT table                    |
| `iptables -A INPUT -p tcp --dport 80 -j ACCEPT`      | Allow TCP 80                      |
| `iptables -D INPUT 1`                                | Delete rule by number             |
| `iptables-save`                                      | Dump iptables config              |
| `iptables-restore < rules`                           | Restore rules                     |
| `nft list ruleset`                                   | Show nftables rules               |
| `nft add rule inet filter input tcp dport 22 accept` | Allow SSH via nft                 |
| `ip rule show`                                       | Policy routing rules              |
| `ip route show table all`                            | All routing tables                |
| `ip route get 8.8.8.8`                               | Route lookup                      |
| `ip link set eth0 mtu 1400`                          | Change MTU                        |
| `ethtool -k eth0`                                    | NIC offload features              |
| `ethtool -K eth0 tso off gso off`                    | Disable offloading                |
| `ss -i`                                              | TCP internal stats                |
| `tc qdisc show`                                      | Traffic control status            |
| `conntrack -L`                                       | View connection tracking          |
| `dropwatch -l kas`                                   | Kernel packet drop tracing        |
| `tcpdump -nnvvXSs 0`                                 | Deep packet capture               |
| `ip -s link`                                         | Interface error counters          |
