# Azure Common Components in Graphviz  

## Templates for Key Azure Services

## 1. Networking Components

### Virtual Network with Subnets

```dot
digraph VNetExample {
    rankdir=TB;
    
    subgraph cluster_vnet {
        label="vnet-prod (10.0.0.0/16)";
        bgcolor="lightgray";
        
        subgraph cluster_subnet1 {
            label="snet-web (10.0.1.0/24)";
            AppService [shape=box3d];
            APIM [shape=box3d];
        }
        
        subgraph cluster_subnet2 {
            label="snet-data (10.0.2.0/24)";
            SQL [shape=cylinder];
            Redis [shape=box3d];
        }
    }
    
    AppService -> SQL [label="TCP 1433"];
}
```

Key Features:

- `cluster_*` for visual grouping
- IP range labels
- Protocol-specific ports

### Load Balancers

```dot
digraph LBDiagram {
    User -> FrontDoor [shape=pentagon];
    FrontDoor -> {AppEast, AppWest} [label="Round Robin"];
}
```

---

## 2. Compute Services

### VM Scale Set

```dot
digraph VMSS {
    subgraph cluster_scale_set {
        label="VMSS (Linux)";
        VM1, VM2, VM3 [shape=box3d];
    }
    LoadBalancer -> {VM1, VM2, VM3};
}
```

### Container Services

```dot
digraph AKS {
    subgraph cluster_aks {
        label="AKS Cluster";
        Pod1, Pod2 [shape=note];
    }
    Pod1 -> CosmosDB [shape=cylinder];
}
```

---

## 3. Storage & Databases

### Storage Account Services

```dot
digraph StorageExample {
    rankdir=LR;
    
    App -> {
        Blob [shape=folder, label="Blob Storage"],
        Queue [shape=box, label="Queue"],
        Table [shape=plaintext, label="Table"]
    };
}
```

### Database Tier

```dot
digraph DatabaseTier {
    App -> {
        SQL [shape=cylinder, label="Azure SQL"],
        Cosmos [shape=cylinder, label="Cosmos DB"],
        PostgreSQL [shape=cylinder, label="Flexible Server"]
    };
}
```

---

## 4. Security Components

### NSG Rules

```dot
digraph NSGExample {
    Internet [shape=cloud];
    WebApp [shape=box3d];
    
    Internet -> WebApp [label="Allow TCP 443", color="green"];
    Internet -> WebApp [label="Deny TCP 22", color="red", style=dashed];
}
```

### Private Endpoints

```dot
digraph PrivateLink {
    App -> PE [label="Private Endpoint", style=dotted];
    PE -> Storage [shape=folder];
}
```

---

## 5. Complete Azure Architecture

```dot
digraph FullArchitecture {
    rankdir=TB;
    
    // Networking
    subgraph cluster_vnet {
        label="Hub Network";
        Firewall [shape=doubleoctagon];
    }
    
    // Compute
    subgraph cluster_spoke1 {
        label="Spoke 1";
        AKS [shape=note];
        Functions [shape=box3d];
    }
    
    // Data
    subgraph cluster_data {
        label="Data Tier";
        CosmosDB [shape=cylinder];
        Blob [shape=folder];
    }
    
    // Connections
    Firewall -> AKS [label="Egress Control"];
    Functions -> CosmosDB;
    AKS -> Blob;
}
```

---

## Best Practices

1. Consistent Naming:

   ```bash
   // Good
   AppService_Prod [shape=box3d];
   
   // Avoid
   App1 [shape=box];
   ```

2. Color Coding:

   ```bash
   node [color="#0078D4"]; // Azure blue
   security [color="#FF0000"]; // Red for security
   ```

3. Layered Outputs:

   ```bash
   # Generate multiple formats
   dot -Tpng arch.dot -o arch.png
   dot -Tsvg arch.dot -o arch.svg
   ```
