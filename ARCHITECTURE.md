# Schéma d'Architecture du Portail Captif

```mermaid
flowchart TD
    subgraph "Utilisateur WiFi"
        User[Utilisateur\nSmartphone / Ordinateur] -->|Connexion WiFi| AP[Point d'Accès WiFi\nMode AP]
    end

    AP -->|DHCP + DNS| Server[Serveur Linux Ubuntu 24.04\nGateway / Captive Portal]

    subgraph "Serveur Linux - Infrastructure"
        direction TB
        
        dnsmasq[dnsmasq\nDHCP + DNS Redirection]
        nft[nftables\nRègles de redirection captive]
        
        Nginx[Nginx\nReverse Proxy + HTTPS]
        Flask[Flask\nPortail Captif Web]
        
        FreeRADIUS[FreeRADIUS\nAuthentification]
        Postgres[(PostgreSQL\nBase de données)]
        
        dnsmasq --> nft
        nft --> Nginx
        Nginx --> Flask
        Flask <--> Postgres
        Flask --> FreeRADIUS
        FreeRADIUS <--> Postgres
    end

    subgraph "Flux Authentification"
        Flask -->|Requête RADIUS| FreeRADIUS
        FreeRADIUS -->|Succès| Flask
        Flask -->|Ajout MAC/IP| nft
    end

    subgraph "Accès Internet"
        Internet[Internet / Réseau Externe]
    end

    nft -->|Après authentification\nTrafic autorisé| Internet

    style Server fill:#1e3a8a,stroke:#60a5fa,stroke-width:3px,color:#fff
    style User fill:#166534,stroke:#4ade80