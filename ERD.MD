```mermaid
erDiagram

    USERS {
        int id
        string username
        string password_hash
        string email
        int role_id
        datetime created_at
    }

    ROLES {
        int id
        string name
    }

    DEVICES {
        int id
        string mac_address
        string ip_address
        int user_id
    }

    SESSIONS {
        int id
        int user_id
        string ip_address
        datetime start_time
        datetime end_time
        boolean active
    }

    LOGS {
        int id
        int user_id
        string action
        datetime timestamp
    }

    USERS ||--o{ SESSIONS : has
    USERS ||--o{ DEVICES : owns
    USERS }o--|| ROLES : belongs_to
    USERS ||--o{ LOGS : generates