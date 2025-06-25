# Active Directory Breaks When Using NAT

In a Proxmox-based lab, using only a single NAT network (via `vmbr0`) often leads to problems with Windows Server Active Directory and DNS, such as:

- Domain join failures
- Clients unable to resolve domain names

These issues occur because NAT is not supported, not tested, and not recommended by [Microsoft](https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/support-for-active-directory-over-nat).

## Solution

To fix this, each VM is configured with two virtual network interfaces:

| Interface | Bridge  | Purpose                     | Gateway |
| --------- | ------- | --------------------------- | ------- |
| NIC 1     | `vmbr0` | NAT/internet access         | Yes     |
| NIC 2     | `vmbr1` | Internal (host-only) AD LAN | None    |

## Setting up

1. **Create a Host-Only (Internal) Bridge in Proxmox**
	- `node-name` -> `Network` -> `Create` -> `Linux Bridge`
	- Name: `vmbr1`
	- IPv4: `192.168.100.1/24`
	- Everything else leave empty
2. **Add 2 Network Interfaces to each VM**
	- `vmbr0` (default Proxmox bridge)
	- `vmbr1` (manual static IP for AD)
3. **Configure Windows Server (Domain Controller) for `vmbr1` (Internal Bridge)**
	- IP: `192.168.100.5`
	- Subnet Mask: `255.255.255.0`
	- Default Gateway: `192.168.100.1` (or leave blank)
	- DNS: `192.168.100.5`
4. **Configure Windows 11 Client for `vmbr1` (Internal Bridge)**
	- IP: `192.168.100.6`
	- Subnet Mask: `255.255.255.0`
	- Default Gateway: `192.168.100.1` (or leave blank)
	- DNS: `192.168.100.5`
## Notes

- Keep **all domain-related traffic isolated**.
- Don't set the default gateway on `vmbr1`, or clients may try to route internet traffic through the domain controller.