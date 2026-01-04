graph TD;
    subgraph Configuration
        A[Source Repo Config]
        B[Target System Config]
    end

    subgraph "Source System"
        C_Repo[(Documentum Repository)]
    end

    subgraph "Migration Tool"
        D[Export Module]
        E{Import Module}
        F[SharePoint Connector]
        G[Documentum Connector]
    end

    subgraph "Target Systems"
        H_SP[(SharePoint)]
        I_Doc[(Target Documentum)]
    end

    %% Define the flow
    A --> D;
    B --> E;

    C_Repo -- "Reads meta & content" --> D;
    D -- "Extracted Data" --> E;
    E -- "Target: SharePoint" --> F;
    E -- "Target: Documentum" --> G;

    F -- "Writes data" --> H_SP;
    G -- "Writes data" --> I_Doc;

    %% Styling
    style C_Repo fill:#dbf,stroke:#333,stroke-width:2px;
    style H_SP fill:#cff,stroke:#333,stroke-width:2px;
    style I_Doc fill:#dbf,stroke:#333,stroke-width:2px;
