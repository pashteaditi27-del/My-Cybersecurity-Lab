# Cybersecurity Lab Setup

## Week 1 Project: VirtualBox and Kali Linux

### Project Overview
This project documents the setup of a cybersecurity lab using Oracle VirtualBox and Kali Linux on a Windows 11 host machine.

### Tools and Technologies
- Windows 11
- Oracle VirtualBox
- Kali Linux
- NAT Network
- Linux Terminal

### Network Configuration

| Setting | Value |
|---|---|
| NAT Network Name | Kali-NAT |
| Network Subnet | 10.0.0.0/24 |
| Kali Linux IP | 10.0.0.2 |
| Netmask | 255.255.255.0 |
| Gateway | 10.0.0.1 |
| DNS | 8.8.8.8, 1.1.1.1 |

### Lab Setup Tasks
1. Installed Oracle VirtualBox.
2. Imported Kali Linux virtual machine.
3. Configured NAT Network.
4. Assigned a static IP address to Kali Linux.
5. Tested internet connectivity.
6. Enabled bidirectional shared clipboard and drag and drop.
7. Configured a shared folder between Windows and Kali Linux.
8. Created a VirtualBox snapshot named `Week 1 - Kali Setup`.

### Verification Commands
Check IP address:

```bash
ip -4 addr

<divider color="subtle"/>
**Internet connectivity command**
```markdown
Test internet connectivity:

```bash
ping -c 4 google.com

### Screenshots

Screenshots are available in the project repository. They document the VirtualBox configuration, NAT Network, Kali Linux desktop, IP configuration, internet connectivity, and snapshot.

### Conclusion

Successfully configured a working Kali Linux cybersecurity lab environment using Oracle VirtualBox. Verified network connectivity, shared folder access, and VM snapshot creation.

**Project Status:** Week 1 Lab Setup Completed.
