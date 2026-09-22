%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#003366', 'edgeLabelBackground':'#ffffff', 'tertiaryColor': '#fff'}}}%%
graph TD
    %% Define Nodes and Icons (GitHub native Mermaid supports FontAwesome for simple icons)
    classDef virtual fill:#005577,stroke:#333,stroke-width:2px,color:#fff;
    classDef physical fill:#f9f9f9,stroke:#333,stroke-width:1px;

    MacBook[fa:fa-laptop MacBook / User Machine]
    Router[fa:fa-network-wired Home Router / Gateway]

    %% Proxmox Infrastructure
    subgraph ProxmoxVE [fa:fa-server Proxmox VE Host]
        OPNsense[fa:fa-shield-alt OPNsense VM
