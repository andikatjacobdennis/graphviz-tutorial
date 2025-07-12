# Azure Common Components in Graphviz  

## Templates for Key Azure Services

## 1. Networking Components

### Virtual Network with Subnets

```dot
digraph VNetExample {
    rankdir=TB;
    splines=ortho;

    // ===== Global Styles =====
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050", fontcolor="black"];

    // ===== VNET & Subnets =====
    subgraph cluster_vnet {
        label="vnet-prod (10.0.0.0/16)";
        style="rounded,filled";
        bgcolor="#F3F2F1";
        penwidth=1.5;

        // Subnet: Web Tier
        subgraph cluster_web {
            label="snet-web (10.0.1.0/24)";
            style="rounded,filled";
            bgcolor="#E5F6FF";

            node [shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
            appservice [label="App Service"];
            apim       [label="API Management"];
        }

        // Subnet: Data Tier
        subgraph cluster_data {
            label="snet-data (10.0.2.0/24)";
            style="rounded,filled";
            bgcolor="#FFF4F4";

            node [shape=cylinder, fillcolor="#E5E5E5", color="#0078D4", fontcolor="black"];
            sql   [label="Azure SQL DB"];

            node [shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
            redis [label="Azure Cache\nfor Redis"];
        }
    }

    // ===== Connections =====
    appservice -> sql  [label="TCP 1433"];
    apim       -> appservice [label="HTTPS 443"];
}
```

Key Features:

- `cluster_*` for visual grouping
- IP range labels
- Protocol-specific ports

### Load Balancers

```dot
digraph LBDiagram {
    rankdir=TB;
    splines=ortho;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050", fontcolor="black"];

    // User
    User [label="User", shape=oval, style=filled, fillcolor="#0078D4", fontcolor="white"];

    // Front Door (Networking)
    FrontDoor [label="Front Door", shape=pentagon, style=filled, fillcolor="#F3F2F1", color="#0078D4", fontcolor="black"];

    // App Services (Compute)
    node [shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
    AppEast [label="App Service - East"];
    AppWest [label="App Service - West"];

    // Connections
    User -> FrontDoor;
    FrontDoor -> {AppEast AppWest} [label="Round Robin"];
}

```

## 2. Compute Services

### VM Scale Set

```dot
digraph VMSS {
    rankdir=TB;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050", fontcolor="black"];

    // Load Balancer (Networking)
    LoadBalancer [label="Load Balancer", shape=pentagon, style=filled, fillcolor="#F3F2F1", color="#0078D4", fontcolor="black"];

    // VMSS instances (Compute)
    node [shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
    VM1 [label="VM 1"];
    VM2 [label="VM 2"];
    VM3 [label="VM 3"];

    // Cluster to represent the scale set group
    subgraph cluster_scale_set {
        label="VM Scale Set (Linux)";
        style=rounded;
        color=gray;
        VM1; VM2; VM3;
    }

    // Connections
    LoadBalancer -> {VM1 VM2 VM3};
}
```

### Container Services

```dot
digraph AKS {
    rankdir=TB;
    splines=ortho;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050", fontcolor="black"];

    // AKS Pods (Compute - shape=note)
    node [shape=note, style=filled, fillcolor="#0078D4", fontcolor="white"];
    Pod1 [label="Pod 1"];
    Pod2 [label="Pod 2"];

    // AKS cluster grouping
    subgraph cluster_aks {
        label="AKS Cluster";
        style=rounded;
        color=gray;
        Pod1;
        Pod2;
    }

    // Cosmos DB (Database - Azure styling)
    node [shape=cylinder, style=filled, fillcolor="#E5E5E5", color="#0078D4", fontcolor="black"];
    CosmosDB [label="Cosmos DB"];

    // Connections
    Pod1 -> CosmosDB [label="TCP 443"];
}
```

## 3. Storage & Databases

### Storage Account Services

```dot
digraph StorageExample {
    rankdir=LR;
    splines=ortho;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050", fontcolor="black"];

    // Application
    node [shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
    App [label="App Service"];

    // Azure Storage Services (light gray with blue border)
    node [shape=folder, style=filled, fillcolor="#E5E5E5", color="#0078D4", fontcolor="black"];
    Blob  [label="Blob Storage"];
    Queue [label="Queue Storage"];
    Table [label="Table Storage"];

    // Connections
    App -> { Blob Queue Table } [label="REST API"];
}
```

### Database Tier

```dot
digraph DatabaseTier {
    rankdir=LR;
    splines=ortho;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050", fontcolor="black"];

    // Application node (Compute)
    node [shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
    App [label="App Service"];

    // Database nodes
    node [shape=cylinder, style=filled, fillcolor="#E5E5E5", color="#0078D4", fontcolor="black"];
    SQL        [label="Azure SQL DB"];
    Cosmos     [label="Cosmos DB"];
    PostgreSQL [label="PostgreSQL\nFlexible Server"];

    // Connections
    App -> { SQL Cosmos PostgreSQL } [label="ODBC / TCP"];
}
```

## 4. Security Components

### NSG Rules

```dot
digraph NSGExample {
    rankdir=LR;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, fontcolor="black"];

    // Nodes
    Internet [shape=cloud, label="Internet"];

    // Compute (App Service)
    node [shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
    WebApp [label="App Service"];

    // Edges with NSG rules
    Internet -> WebApp [label="Allow TCP 443", color="green", fontcolor="green"];

    Internet -> WebApp [label="Deny TCP 22", color="red", style=dashed, fontcolor="red"];
}
```

### Private Endpoints

```dot
digraph PrivateLink {
    rankdir=LR;
    splines=ortho;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, fontcolor="black"];

    // Application node (compute)
    node [shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
    App [label="App Service"];

    // Private Endpoint (styled note with distinct color)
    node [shape=note, style=filled, fillcolor="#FFF4CE", color="#D83B01", fontcolor="black"];
    PE [label="Private Endpoint"];

    // Storage target
    node [shape=folder, style=filled, fillcolor="#E5E5E5", color="#0078D4", fontcolor="black"];
    Storage [label="Blob Storage"];

    // Edges
    App -> PE [label="Private Link", style=dotted, dir=both, color="#0078D4"];
    PE -> Storage;
}
```

## 5. Complete Azure Architecture

```dot
digraph FullArchitecture {
    rankdir=TB;
    splines=ortho;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, fontcolor="black", color="#505050"];

    // NETWORKING
    node [shape=doubleoctagon, style=filled, fillcolor="#F3F2F1", color="#0078D4", fontcolor="black"];
    subgraph cluster_vnet {
        label="Hub Network";
        Firewall [label="Azure Firewall"];
    }

    // COMPUTE
    node [shape=note, style=filled, fillcolor="#0078D4", fontcolor="white"];
    AKS [label="AKS Cluster"];

    node [shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
    Functions [label="Function App"];

    subgraph cluster_spoke1 {
        label="Spoke 1";
        AKS; Functions;
    }

    // DATA
    node [shape=cylinder, style=filled, fillcolor="#E5E5E5", color="#0078D4", fontcolor="black"];
    CosmosDB [label="Cosmos DB"];

    node [shape=folder, style=filled, fillcolor="#E5E5E5", color="#0078D4", fontcolor="black"];
    Blob [label="Blob Storage"];

    subgraph cluster_data {
        label="Data Tier";
        CosmosDB; Blob;
    }

    // CONNECTIONS
    Firewall -> AKS       [label="Egress Control"];
    Functions -> CosmosDB;
    AKS -> Blob;
}
```

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
