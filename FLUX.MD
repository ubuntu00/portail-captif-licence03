```mermaid
flowchart TD

    A[Utilisateur se connecte au WiFi] --> B[Obtient une adresse IP via DHCP]

    B --> C[Accès à un site web]
    C --> D{Utilisateur authentifié ?}

    D -->|Non| E[Redirection vers Portail Captif]
    E --> F[Affichage page login]

    F --> G[Utilisateur saisit identifiants]
    G --> H[Flask envoie requête à FreeRADIUS]

    H --> I{Identifiants valides ?}

    I -->|Non| J[Afficher erreur]
    J --> F

    I -->|Oui| K[Ajouter IP/MAC dans nftables]
    K --> L[Accès Internet autorisé]

    D -->|Oui| L

    L --> M[Session active]
    M --> N{Session expirée ?}

    N -->|Oui| O[Supprimer accès dans nftables]
    O --> E

    N -->|Non| M