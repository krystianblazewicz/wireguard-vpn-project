# WireGuard VPN on Ubuntu VPS

A personal project documenting the deployment of a WireGuard VPN server on an Ubuntu VPS and its connection to an iOS client.

## Project Goal

Build a working VPN connection and gain practical experience with Linux administration, networking, and troubleshooting.

## Environment

| Component | Role |
| --- | --- |
| Ubuntu VPS | WireGuard VPN server |
| MacBook with macOS | Remote server administration over SSH |
| iPhone | WireGuard VPN client |
| iptables | NAT and packet forwarding rules |
| systemd | VPN service management |

## Implementation

- Connected to the VPS over SSH and updated system packages.
- Installed WireGuard and generated the server keys.
- Created and configured the `wg0` network interface.
- Configured the iPhone as a WireGuard peer.
- Enabled IPv4 forwarding on the server.
- Added iptables MASQUERADE and FORWARD rules.
- Restricted access to private key and configuration files.
- Saved the VPN configuration and enabled the `wg-quick@wg0` service.
- Checked connection handshakes, traffic counters, and service logs.

The iOS client uses `AllowedIPs = 0.0.0.0/0` to route IPv4 traffic through the VPN.

## Troubleshooting

### Private key file access

WireGuard returned a permission error when loading the private key from its original location.

Loading the key succeeded after it was copied to `/etc/wireguard` with restricted permissions. The exact cause of the original access restriction was not conclusively established.

### Peer configuration

Corrected the server-side peer configuration to use the client's VPN address, `10.0.0.2/32`, and removed an unintended peer entry.

### Existing interface conflict

Starting the systemd service failed because `wg0` had already been created manually.

I inspected the service logs, saved the configuration, and brought down the existing interface before starting it through systemd.

## Verified Results

- A recent WireGuard handshake was visible on the iPhone.
- The client showed traffic sent and received through the tunnel.
- The server service reported `active (exited)` with a successful exit status.
- The VPN service was enabled for automatic startup.

These observations confirm an operational tunnel. They do not constitute a complete security or performance assessment.

## Further Testing

- Confirm that the client's public IPv4 address matches the VPS.
- Verify DNS behaviour while connected.
- Check IPv6 behaviour separately.
- Test automatic recovery after a VPS restart.
- Compare latency and download/upload speeds with and without the VPN.
- Review the final firewall rules.

## Skills Practised

- Linux command-line administration
- Remote administration over SSH
- WireGuard configuration
- IPv4 addressing and routing
- NAT and iptables rules
- File permissions
- systemd service management
- Troubleshooting using logs and connection statistics
