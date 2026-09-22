
# Home Lab & Private Cloud Architecture
A Proxmox-based home lab that doubles as a family private cloud and a self-contained offensive-security practice environment. A single physical host runs everything: firewall, DNS/DHCP, remote access, file storage, and an isolated attack range, segmented into separate virtual network zones so the practice environment can't reach the family-facing services.
![Homelab Architecture](./images/image2-light.svg)

## Components
 
| Component | Type | Role |
|---|---|---|
| **OPNsense** | VM | Firewall/router between zones; owns the uplink and routes traffic into the LAN and OPT1 zones |
| **Pi-hole** | LXC | DHCP + DNS for the home-network segment (`vmbr0`); blocks ads and trackers network-wide for anything on that segment |
| **Tailscale** | LXC | WireGuard-based mesh VPN; advertises the `10.0.0.0/24` subnet so authenticated remote devices can reach the lab from anywhere |
| **Kali Linux** | LXC | Isolated offensive-security container, sandboxed on its own bridge for attack simulation and tool practice |
| **App Containers** | Docker / LXC | General-purpose services running in the "target" zone |
| **Storage** | Service | Family private cloud (Immich, Nextcloud, etc.) for photos and files |

## Components
 
| Component | Type | Role |
|---|---|---|
| **OPNsense** | VM | Firewall/router between zones; owns the uplink and routes traffic into the LAN and OPT1 zones |
| **Pi-hole** | LXC | DHCP + DNS for the home-network segment (`vmbr0`); blocks ads and trackers network-wide for anything on that segment |
| **Tailscale** | LXC | WireGuard-based mesh VPN; advertises the `10.0.0.0/24` subnet so authenticated remote devices can reach the lab from anywhere |
| **Kali Linux** | LXC | Isolated offensive-security container, sandboxed on its own bridge for attack simulation and tool practice |
| **App Containers** | Docker / LXC | General-purpose services running in the "target" zone |
| **Storage** | Service | Family private cloud (Immich, Nextcloud, etc.) for photos and files |
