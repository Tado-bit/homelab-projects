# pfSense Rule Order Explained

- pfSense evaluates rules top-down
- First matching rule is applied
- An implicit deny exists at the bottom

Important notes:
- "LAN net" = subnet of the current interface
- "LAN subnets" = all RFC1918 private networks
- Using "LAN subnets" can unintentionally block internet access

Solution:
- Use interface-specific nets and custom aliases

