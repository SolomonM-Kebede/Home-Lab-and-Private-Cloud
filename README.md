graph TD
    %% Custom Styles
    classDef virtual fill:#005577,stroke:#333,stroke-width:2px,color:#fff;
    classDef physical fill:#f9f9f9,stroke:#333,stroke-width:2px,color:#333;
    classDef cloud fill:#006644,stroke:#333,stroke-width:2px,color:#fff;

    %% External Devices
    MacBook["💻 MacBook (Admin / User)"]
    Router["🌐 Home Router (Gateway)"]

    %% Proxmox Infrastructure
    subgraph ProxmoxVE ["🖥️ Proxmox VE Host (Hybrid Lab & Cloud)"]
        
        OPNsense["🛡️ OPNsense Firewall VM
IP: 10.0.0.x:9090"]:::virtual

    subgraph AttackerZone ["Offensive Security Zone"]
        Kali["💀 Kali Linux Container

IP: 10.0.10.x"]:::virtual
end

     subgraph PrivateCloud ["Family Private Cloud"]
        Storage["☁️ Media, Files & Photo Storage

(Immich, Nextcloud, etc.)"]:::cloud
end

    %% Virtual Switches (Bridges)
    subgraph Bridges ["Virtual Network Bridges"]
        vmbr0("vmbr0

(WAN / Internet)"):::physical
vmbr1("vmbr1

(LAN / Attacker)"):::physical
vmbr2("vmbr2

(OPT1 / Target)"):::physical
end
end

    %% Network Connections
    MacBook -. "SSH Tunnel (localhost:9090)" .-> Router
    Router -->|"Home Network"| vmbr0
    
    vmbr0 -->|"net0 (WAN)"| OPNsense
    
    OPNsense -->|"net1 (LAN)"| vmbr1
    vmbr1 --> Kali
    vmbr1 --> PrivateCloud
    
    OPNsense -->|"net2 (OPT1)"| vmbr2
    vmbr2 --> Containers
    
    %% Link Styling (Makes the SSH tunnel stand out)
    linkStyle 0 stroke-width:3px,stroke-dasharray: 5 5,stroke:#d97706;

### How GitHub renders this:
* **Emojis:** Are used instead of external FontAwesome icons because emojis are 100% natively supported across all devices and GitHub's dark/light modes without requiring external plugins.
* **Colors:** The `classDef` lines apply dark blue to your cybersecurity lab VMs, green to your private cloud storage, and light gray to the virtual switches, making the architecture visually distinct.
* **The Orange Dotted Line:** The `linkStyle` explicitly colors your SSH tunnel path so readers instantly understand how you manage the OPNsense instance securely from your MacBook.
