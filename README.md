
# Home Lab & Private Cloud Architecture
```mermaid
flowchart TB
 subgraph CoreServices["🧩 Core Network & Security Services"]
        OPNsense["🛡️ OPNsense Firewall VM<br>IP: 10.0.0.1:9090"]
        Pihole["🕳️ Pi-hole LXC<br>DHCP + DNS Ad-Block<br>IP: 10.0.0.2"]
        Tailscale["🔒 Tailscale LXC<br>Subnet Router<br>IP: 10.0.0.5"]
  end
 subgraph AttackerZone["Offensive Security Zone"]
        Kali["💀 Kali Linux Container<br>IP: 10.0.10.x"]
  end
 subgraph PrivateCloud["Family Private Cloud"]
        Storage["☁️ Media, Files &amp; Photo Storage<br>(Immich, Nextcloud, etc.)"]
  end
 subgraph TargetZone["Target / Services Zone"]
        Containers["📦 App Containers<br>(Docker / LXC)"]
  end
 subgraph Bridges["Virtual Network Bridges"]
        vmbr0("vmbr0<br>WAN / Internet")
        vmbr1("vmbr1<br>LAN / Attacker")
        vmbr2("vmbr2<br>OPT1 / Target")
  end
 subgraph ProxmoxVE["🖥️ Proxmox VE Host (Hybrid Lab & Cloud)"]
        CoreServices
        AttackerZone
        PrivateCloud
        TargetZone
        Bridges
  end
 subgraph Legend["Legend"]
    direction LR
        L1["VM"]
        L2["LXC Container"]
        L3["Bridge / Interface"]
        L4["Private Cloud Service"]
        L5["External / Remote"]
  end
    MacBook["💻 MacBook (Admin / User)"] -. SSH Tunnel (localhost:9090) .-> Router["🌐 Home Router (Gateway)"]
    RemoteDevices["🌍 Remote Devices<br>(Phone, Work Laptop, etc.)"] L_RemoteDevices_Tailscale_0@-. Tailscale (WireGuard Mesh) .-> Tailscale
    Router L_Router_vmbr0_0@-- Home Network --> vmbr0
    vmbr0 -- DHCP + DNS --> Pihole
    vmbr0 -- net0 (WAN) --> OPNsense
    vmbr0 -- Direct Access --> Storage
    Tailscale -. "Subnet Route<br>10.0.0.0/24" .-> vmbr0
    OPNsense -- net1 (LAN) --> vmbr1
    vmbr1 --> Kali
    OPNsense -- net2 (OPT1) --> vmbr2
    vmbr2 --> Containers

     OPNsense:::vm
     Pihole:::lxc
     Tailscale:::lxc
     Kali:::lxc
     Storage:::cloud
     Containers:::lxc
     vmbr0:::physical
     vmbr1:::physical
     vmbr2:::physical
     L1:::vm
     L2:::lxc
     L3:::physical
     L4:::cloud
     L5:::external
     MacBook:::external
     Router:::external
     RemoteDevices:::external
    classDef vm fill:#005577,stroke:#333,stroke-width:2px,color:#fff
    classDef lxc fill:#0e7490,stroke:#333,stroke-width:2px,color:#fff
    classDef physical fill:#f9f9f9,stroke:#333,stroke-width:2px,color:#333
    classDef cloud fill:#006644,stroke:#333,stroke-width:2px,color:#fff
    classDef external fill:#78350f,stroke:#333,stroke-width:2px,color:#fff
    linkStyle 0 stroke-width:3px,stroke-dasharray: 5 5,stroke:d97706,fill:none
    linkStyle 1 stroke-width:3px,stroke-dasharray: 5 5,stroke:2563eb,fill:none
    linkStyle 5 stroke-width:2px,stroke-dasharray: 3 3,stroke:2563eb,fill:none

    L_RemoteDevices_Tailscale_0@{ animation: none } 
    L_Router_vmbr0_0@{ animation: slow }
```

