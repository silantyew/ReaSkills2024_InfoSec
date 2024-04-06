# Файервол в Linux
### Iptables

**Overview of iptables:**

Iptables stands as a cornerstone tool for controlling firewalls on Linux systems. Operating directly with the Linux kernel for packet filtering, iptables provides a versatile but verbose interface.

**Organizational Structure:**

The organizational structure of iptables involves tables, chains, rules, and targets. Three primary tables – filter, nat, and mangle – categorize rules. The filter table manages incoming and outgoing packets, nat facilitates Network Address Translation (NAT), and mangle is employed for advanced packet alteration.

**Default Policies and Rule Creation:**

By default, iptables adds rules to the filter table, with default policies for INPUT, OUTPUT, and FORWARD chains set to ACCEPT. Security best practices recommend setting at least FORWARD and INPUT policies to DROP. Loopback interface access is usually allowed, and established or related connections are accepted.

### Nftables

**Overview of nftables:**

Nftables emerges as a user-friendly alternative to iptables, offering a more logical and streamlined structure. While positioned as a replacement for iptables, both tools coexist in modern systems.

**Organizational Structure in nftables:**

Nftables adopts a logical structure comprising tables, chains, rules, and verdicts. It simplifies rule organization with various table types, including ip, arp, ip6, bridge, inet, and netdev.

### UFW

Uncomplicated Firewall (ufw) serves as a frontend for iptables, offering a simplified interface for managing firewall configurations. It is designed to be user-friendly and automatically sets up iptables rules based on specified configurations. Ufw not only simplifies iptables but also integrates well with applications and services. Its simplicity makes it an ideal choice for those who want a quick setup without delving into intricate firewall configurations. Moreover, ufw supports application profiles, allowing users to define rules specific to applications.

### Firewalld

Firewalld streamlines dynamic firewall configuration, featuring zones to declare trust levels in interfaces and networks. It comes pre-installed in distributions like Red Hat Enterprise Linux, Fedora, CentOS, and can be installed on others. Firewalld excels in dynamic environments where network configurations change frequently. Its zone-based approach allows administrators to define different trust levels for various network interfaces. Firewalld works with both iptables and nftables.