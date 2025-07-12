# **Simple Azure Diagrams in Graphviz**  

## **1. Starter Template: Single Azure Service**  

### **Basic App Service Diagram**  

```dot
digraph AppService {
    rankdir="LR"; // Left-to-right flow
    splines=ortho;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050", fontcolor="black"];

    // Identity/User
    node [shape=oval, style=filled, fillcolor="#0078D4", fontcolor="white"];
    User [label="User"];

    // Compute
    node [shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
    App [label="App Service"];

    // Connection
    User -> App [label="HTTPS"];
}
```

**Key Elements:**  

- `box3d`: Standard Azure resource shape  
- `fillcolor="#0078D4"`: Microsoft Azure blue  
- Explicit protocol labeling  

## **2. Adding a Database Tier**  

### **Web App + Database**  

```dot
digraph WebAppWithDB {
    rankdir="TB"; // Top-to-bottom layout
    
    // Components
    User [shape=oval];
    WebApp [shape=box3d, label="App Service", fillcolor="#0078D4", style=filled, fontcolor=white];
    SQL [shape=cylinder, label="Azure SQL", fillcolor="#F5F5F5", style=filled];
    
    // Connections
    User -> WebApp [label="HTTPS (443)"];
    WebApp -> SQL [label="TCP 1433", fontsize=8];
    
    // Styling
    edge [color="#505050"];
}
```

**Pro Tip:** Use **port numbers** for clarity in security reviews.

## **3. Including Storage**  

### **Three-Tier Architecture**  

```dot
digraph ThreeTier {
    rankdir="LR";
    splines=ortho;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050", fontcolor="black"];

    // Frontend (User & CDN)
    node [shape=oval, style=filled, fillcolor="#0078D4", fontcolor="white"];
    Browser [label="Browser"];

    node [shape=box, style="rounded,filled", fillcolor="#F3F2F1", color="#0078D4", fontcolor="black"];
    CDN [label="Azure CDN"];

    // Middle Tier (App Service)
    node [shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
    AppService [label="App Service"];

    // Backend (SQL + Storage)
    node [shape=cylinder, fillcolor="#E5E5E5", color="#0078D4", fontcolor="black"];
    SQL [label="Azure SQL DB"];

    node [shape=folder, fillcolor="#E5E5E5", color="#0078D4", fontcolor="black"];
    BlobStorage [label="Blob Storage\n(Standard LRS)"];

    // Connections
    Browser -> CDN [label="Static Content"];
    CDN -> AppService;
    AppService -> { SQL BlobStorage };
}
```

## **4. Basic Availability Design**  

### **Load Balanced Web Apps**  

```dot
digraph LoadBalanced {
    rankdir="TB";
    splines=ortho;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050", fontcolor="black"];

    // User (Identity)
    node [shape=oval, style=filled, fillcolor="#0078D4", fontcolor="white"];
    User [label="User"];

    // Front Door (Networking)
    node [shape=pentagon, style="rounded,filled", fillcolor="#F3F2F1", color="#0078D4", fontcolor="black"];
    FrontDoor [label="Azure Front Door"];

    // App Services (Compute)
    node [shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
    App_East [label="App Service\n(East US)"];
    App_West [label="App Service\n(West US)"];

    // Database (Data Layer)
    node [shape=cylinder, fillcolor="#E5E5E5", color="#0078D4", fontcolor="black"];
    CosmosDB [label="Cosmos DB"];

    // Connections
    User -> FrontDoor;
    FrontDoor -> { App_East App_West } [label="Traffic Manager"];
    { App_East App_West } -> CosmosDB [label="Global Distribution"];
}
```

**Visual Cues:**  

- pentagon: Load balancer shape  
- Region labels for geo-redundancy  

## **5. Adding Security Elements**  

### **With Network Restrictions**  

```dot
digraph WithSecurity {
    rankdir=TB;
    splines=ortho;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050", fontcolor="black"];

    // Internet (external)
    node [shape=cloud, style=filled, fillcolor=white, fontcolor=black];
    Internet;

    // Azure Firewall (Security)
    node [shape=doubleoctagon, style=filled, fillcolor="#FFD6D6", color="#D83B01", fontcolor="black"];
    Firewall [label="Azure Firewall"];

    // App Service (Compute)
    node [shape=box3d, style=filled, fillcolor="#0078D4", fontcolor=white];
    AppService [label="App Service"];

    // SQL Database (Database tier)
    node [shape=cylinder, fillcolor="#E5E5E5", color="#0078D4", fontcolor="black"];
    SQL [label="Azure SQL DB"];

    // Connections
    Internet -> Firewall;
    Firewall -> AppService [label="Allow 443"];

    AppService -> SQL [label="Private Endpoint", style=dotted];
    Internet -> SQL [label="Deny Public Access", color="red", style=dashed];
}
```

## **Common Modifications Table**  

| Scenario | Graphviz Code Snippet |  
|----------|-----------------------|  
| **Add Monitoring** | `App -> LogAnalytics [label="Diagnostics", style=dashed]` |  
| **Private Link** | `Storage -> PE [label="Private Endpoint", dir=both]` |  
| **Zone Redundancy** | `SQL [label="Azure SQL\nZone-Redundant"]` |  

## **Try These Exercises**  

1. **Modify the Three-Tier diagram** to include Redis Cache  
2. **Create a zone-redundant** storage diagram  
3. **Add NSG rules** to the security example  

**Solution for #1:**  

```dot
digraph AppService {
    rankdir="LR"; // Left-to-right layout
    splines=ortho;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050", fontcolor="black"];

    // User (identity)
    node [shape=oval, style=filled, fillcolor="#0078D4", fontcolor="white"];
    User [label="User"];

    // App Service (compute)
    node [shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
    App [label="App Service"];

    // Redis Cache (database/caching)
    node [shape=cylinder, fillcolor="#E5E5E5", color="#0078D4", fontcolor="black"];
    Redis [label="Azure Cache\nfor Redis"];

    // Connections
    User -> App [label="HTTPS"];
    App -> Redis [label="TCP 6379"];
}
```

**Solution for #2:**  

```dot
digraph ZoneRedundantStorage {
    rankdir="TB";
    splines=ortho;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050", fontcolor="black"];

    // App Service (compute)
    node [shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
    AppService [label="App Service"];

    // Blob Storage (storage tier)
    node [shape=folder, fillcolor="#E5E5E5", color="#0078D4", fontcolor="black"];

    subgraph cluster_region {
        label="East US (Zones 1-2)";
        style=dashed;

        subgraph cluster_zone1 {
            label="Zone 1";
            style=rounded;
            Storage1 [label="Blob Storage\n(Zone 1)"];
        }

        subgraph cluster_zone2 {
            label="Zone 2";
            style=rounded;
            Storage2 [label="Blob Storage\n(Zone 2)"];
        }
    }

    // Connections
    AppService -> {Storage1 Storage2} [label="ZRS Access", style=dashed];

    // Legend
    node [shape=plaintext];
    Legend [label="ZRS = Zone-Redundant Storage"];
}
```

**Solution for #3:**  

```dot
digraph SecurityWithNSG {
    rankdir=TB;
    splines=ortho;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050", fontcolor="black"];

    // Public node
    Internet [shape=cloud];

    // Azure Firewall (security)
    node [shape=doubleoctagon, style=filled, fillcolor="#FFD6D6", color="#D83B01", fontcolor="black"];
    Firewall [label="Azure Firewall"];

    // Compute - App Service
    node [shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
    AppService [label="App Service"];

    // Database - Azure SQL
    node [shape=cylinder, fillcolor="#E5E5E5", color="#0078D4", fontcolor="black"];
    SQL [label="Azure SQL"];

    // NSG rules (visual reference only)
    node [shape=note, style=filled, fillcolor="#FFF4CE", color="#D83B01", fontcolor="black"];
    subgraph cluster_nsg {
        label="NSG Rules";
        bgcolor="#FFF4E5";
        style=dashed;
        rule1 [label="Allow HTTPS\nFrom: Internet\nTo: App Service\nPriority: 100"];
        rule2 [label="Deny SSH\nFrom: *\nTo: *\nPriority: 200"];
    }

    // Connections
    Internet -> Firewall;
    Firewall -> AppService [label="Port 443", color="green"];
    AppService -> SQL [label="Private Endpoint", style=dotted];
    Internet -> SQL [label="Blocked", color="red", style=dashed];

    // NSG rules as notes (visual grouping only)
    rule1 -> AppService [style=invis];
    rule2 -> Internet [style=invis];
}

```

**Pro Tip:** Use this command to generate high-res diagrams:  

```bash
dot -Tpng -Gdpi=300 diagram.dot -o diagram.png
```
