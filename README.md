Dnsmasq
=========

Installs and configures Dnsmasq to serve as local DNS cache and to resolve local domains for given hosts. The DHCP and TFTP functions are not activated.

When I looked for an existing ansible role which installs and configures Dnsmasq, I had two main problems.
1. The role was too powerful for my taste. There were so many options but I just needed a simple local DNS server.
2. The role was abandoned and not maintained anymore.

So this role is kept as simple as possible. You can set the upstream DNS servers and configure DNS records for local hosts.

Requirements & Dependencies
---------------------------

None at all

Role Variables
--------------

Every variable is optional, none has to be set.
| Variable | Description |
|-|-|
| `dns_resolvers` | All IP addresses of the upstream DNS servers you want to use for requests the local Dnsmasq server can't answer. Defaults to `['9.9.9.9', '142.112.112.112'] |
| `single_hosts` | All hosts with the full domainname and their respective IP addresses. |
| `local_domains` | Can be used instead of `single_hosts` when multiple hosts belong to the same domain, so you don't have to repeat the domain for each and every host. The full domain name of each host is determined by combining the hostname and the given domain. |
| `dnsmasq_resolve_conf` | Sets the directory where the `resolve.conf` will be saved. Defaults to `/etc/dnsmasq.d/resolv.conf`. |
| `dnsmasq_hosts` | Sets the directory where the `hosts` will be saved. Defaults to `/etc/dnsmasq.d/hosts`. |

Example Playbook
----------------

The following playbook configures the Quad9 DNS servers as upstream servers. Also it sets the domainname for the router and multiple servers which belong to different domains.

    - hosts: localhost
      roles:
      - role: bytesturm.simple-dnsmasq
        vars:
          dnsmasq_resolve_conf: /etc/dnsmasq.d/resolv.conf
          dnsmasq_hosts: /etc/dnsmasq.d/hosts
          dns_resolvers:
            - 9.9.9.9
            - 149.112.112.112
          single_hosts:
            - host: fritz.box
              ip: 192.168.178.1
          local_domains:
            - domain: 'home.example.com'
              hosts:
              - host: 'server1'
                ip: '192.168.2.101'
              - host: 'server2'
                ip: '192.168.2.102'
            - domain: 'homelab.local'
              hosts:
              - host: 'clusternode1'
                ip: '192.168.2.111'
              - host: 'clusternode2'
                ip: '192.168.2.112'
              - host: 'clusternode3'
                ip: '192.168.2.113'
              - host: 'clusternode4'
                ip: '192.168.2.114'
              - host: 'clusternode5'
                ip: '192.168.2.115'

License
-------

MIT License - Copyright &copy; 2021 ByteSturm
