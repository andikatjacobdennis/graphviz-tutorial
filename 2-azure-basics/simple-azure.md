# **Simple Azure Diagrams in Graphviz**  

## **1. Starter Template: Single Azure Service**  

### **Basic App Service Diagram**  

```dot
digraph AppService {
    rankdir="LR"; // Left-to-right flow
    User [shape=oval];
    App [shape=box3d, label="App Service", fillcolor="#0078D4", style=filled, fontcolor=white];
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

    // Style
    node [fontname="Segoe UI"];

    // Frontend
    Browser -> CDN [label="Static Content"];

    // Middle Tier
    CDN -> AppService;

    // Backend
    AppService -> { SQL BlobStorage };

    // Node styling
    SQL [shape=cylinder];
    BlobStorage [shape=folder, label="Blob Storage\n(Standard LRS)"];
}
```

## **4. Basic Availability Design**  

### **Load Balanced Web Apps**  

```dot
digraph LoadBalanced {
    rankdir="TB";

    node [shape=box]; // Default node shape

    // Define specific node shapes
    FrontDoor [shape=pentagon];
    App_East [label="East US"];
    App_West [label="West US"];

    // Diagram flow
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
    // Public components
    Internet -> Firewall [shape=doubleoctagon];
    
    // Private components
    Firewall -> AppService [label="Allow 443"];
    AppService -> SQL [label="Private Endpoint", style=dotted];
    
    // Denied path
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
    rankdir="LR"; // Left-to-right flow
    User [shape=oval];
    App [shape=box3d, label="App Service", fillcolor="#0078D4", style=filled, fontcolor=white];
    User -> App [label="HTTPS"];
    App -> Redis [shape=box3d, label="Cache", color="#FF6600"];
}
```

**Solution for #2:**  

```dot
digraph ZoneRedundantStorage {
    rankdir="TB";
    
    // Availability Zones
    subgraph cluster_region {
        label="East US (Zones 1-3)";
        
        subgraph cluster_zone1 {
            label="Zone 1";
            Storage1 [shape=folder, label="Blob Storage\n(Zone 1)"];
        }
        
        subgraph cluster_zone2 {
            label="Zone 2";
            Storage2 [shape=folder, label="Blob Storage\n(Zone 2)"];
        }
    }
    
    // Client access
    AppService -> {Storage1, Storage2} [label="Zone-Redundant\n(LRS ZRS)", style=dashed];
    
    // Legend
    Legend [shape=plaintext, label="ZRS = Zone-Redundant Storage", fontsize=10];
}
```

**Solution for #3:**  

```dot
digraph SecurityWithNSG {
    // Public components
    Internet -> Firewall [shape=doubleoctagon];
    
    // Private components
    subgraph cluster_nsg {
        label="NSG Rules";
        bgcolor="#FFF4E5";
        
        rule1 [shape=note, label="Allow HTTPS\nFrom: Internet\nTo: AppService\nPriority: 100"];
        rule2 [shape=note, label="Deny SSH\nFrom: *\nTo: *\nPriority: 200"];
    }
    
    Firewall -> AppService [label="Port 443", color="green"];
    AppService -> SQL [label="Private Endpoint", style=dotted];
    
    // Denied path
    Internet -> SQL [label="Blocked", color="red", style=dashed];
    
    // Connect NSG to components
    rule1 -> AppService [style=invis];
    rule2 -> Internet [style=invis];
}
```

**Pro Tip:** Use this command to generate high-res diagrams:  

```bash
dot -Tpng -Gdpi=300 diagram.dot -o diagram.png
```
