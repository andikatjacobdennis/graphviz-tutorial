# Mastering Subgraphs for Azure Diagrams  

## 1. Basic Subgraph Syntax  

### Virtual Network Container  

```dot
digraph VNetExample {
    rankdir=LR;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=10, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, color="#505050", penwidth=1.0];

    // Internet node
    Internet [shape=cloud, label="Internet"];

    // Virtual Network
    subgraph cluster_vnet {
        label="Virtual Network: vnet-prod (10.0.0.0/16)";
        style="rounded,filled";
        bgcolor="#F3F2F1";
        color="#0078D455";

        // Web subnet
        subgraph cluster_web {
            label="snet-web (10.0.1.0/24)";
            style="filled";
            bgcolor="white";

            AppService [label="App Service", shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
        }

        // Data subnet
        subgraph cluster_data {
            label="snet-data (10.0.2.0/24)";
            style="filled";
            bgcolor="white";

            SQL [label="Azure SQL DB", shape=cylinder, style=filled, fillcolor="#E5E5E5", color="#0078D4"];
        }
    }

    // Security layer
    NSG [label="NSG", shape=note, style=filled, fillcolor="#FFF4CE", color="#D83B01"];

    // Connections
    Internet -> AppService [label="HTTPS", color="#0078D4"];
    AppService -> SQL [label="TCP 1433", color="#505050"];
    NSG -> AppService [style=invis]; // logical association
}

```

Key Features:  

- `cluster_` prefix triggers visual containment  
- Customizable label position (`labeljust`, `labelloc`)  
- Background/border styling  

## 2. Azure-Specific Groupings  

### Resource Group Organization  

```dot
digraph ResourceGroups {
    rankdir=LR;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=10, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, color="#505050", penwidth=1.0];

    // Production Resource Group
    subgraph cluster_rg_prod {
        label="Resource Group: RG-Production";
        style="rounded,filled";
        bgcolor="#F3F2F1";
        color="#0078D455";

        AppService [label="App Service\n(Prod)", shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
        SQL [label="Azure SQL\n(P1)", shape=cylinder, style=filled, fillcolor="#E5E5E5", color="#0078D4"];
    }

    // Development Resource Group
    subgraph cluster_rg_dev {
        label="Resource Group: RG-Development";
        style="rounded,filled";
        bgcolor="#FFF4CE";
        color="#D83B0155";

        DevApp [label="App Service\n(Dev)", shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
        DevSQL [label="Azure SQL\n(Basic)", shape=cylinder, style=filled, fillcolor="#E5E5E5", color="#0078D4"];
    }

    // Optional: logical association
    AppService -> SQL [style=dashed, label="connects to"];
    DevApp -> DevSQL [style=dashed, label="connects to"];
}
```

### Availability Zone Layout  

```dot
digraph ZonalArchitecture {
    rankdir=TB;

    // Global styles
    graph [fontname="Segoe UI"];
    node  [fontname="Segoe UI", fontsize=10, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=9, color="#505050", penwidth=1.0];

    // Load Balancer (Networking)
    LoadBalancer [label="Load Balancer", shape=pentagon, style="rounded,filled", fillcolor="#F3F2F1", color="#0078D4", fontcolor="black"];

    // Zone 1
    subgraph cluster_zone1 {
        label="Availability Zone 1";
        style="rounded,filled";
        bgcolor="#E5F1FB";
        color="#0078D455";

        App1 [label="App Service\n(Zone 1)", shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
        Data1 [label="Azure SQL\n(Zone 1)", shape=cylinder, style=filled, fillcolor="#E5E5E5", color="#0078D4"];
    }

    // Zone 2
    subgraph cluster_zone2 {
        label="Availability Zone 2";
        style="rounded,filled";
        bgcolor="#E5F1FB";
        color="#0078D455";

        App2 [label="App Service\n(Zone 2)", shape=box3d, style=filled, fillcolor="#0078D4", fontcolor="white"];
        Data2 [label="Azure SQL\n(Zone 2)", shape=cylinder, style=filled, fillcolor="#E5E5E5", color="#0078D4"];
    }

    // Connections
    LoadBalancer -> {App1 App2} [label="Zonal Routing"];
    App1 -> Data1 [style=dashed, label="Local DB"];
    App2 -> Data2 [style=dashed, label="Local DB"];
}
```

## 3. Nested Subgraphs  

### Hub-Spoke Network  

```dot
digraph HubSpoke {
    rankdir=TB;

    // Global styles
    graph [fontname="Segoe UI"];
    node  [fontname="Segoe UI", fontsize=10, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=9, color="#505050"];

    // Hub Network
    subgraph cluster_hub {
        label="Hub Virtual Network";
        style="rounded,filled";
        bgcolor="#E5F1FB";
        color="#0078D455";

        Firewall [label="Azure Firewall", shape=doubleoctagon, style=filled, fillcolor="#D83B01", fontcolor=white];

        subgraph cluster_shared {
            label="Shared Services";
            style="rounded";
            Monitor [label="Azure Monitor", shape=box3d, style=filled, fillcolor="#F3F2F1"];
            DNS [label="Azure DNS", shape=box3d, style=filled, fillcolor="#F3F2F1"];
        }
    }

    // Spoke Network
    subgraph cluster_spoke1 {
        label="Spoke 1: Workload VNet";
        style="rounded,filled";
        bgcolor="#FFFFFF";
        color="#0078D455";

        AppService [label="App Service", shape=box3d, style=filled, fillcolor="#0078D4", fontcolor=white];
    }

    // Connections
    Firewall -> AppService [label="Traffic Inspection", style=dashed, color="#0078D4"];

    // Optional: monitoring links
    AppService -> Monitor [label="Logs", style=dotted, color=gray];
}

```

## 4. Cross-Subgraph Connections  

### Peering Relationships  

```dot
digraph VNetPeering {
    rankdir=LR;

    // Global styles
    graph [fontname="Segoe UI"];
    node  [fontname="Segoe UI", fontsize=10, style=filled, fillcolor="#F3F2F1", shape=box3d];
    edge  [fontname="Segoe UI", fontsize=9, color="#505050"];

    // VNet East
    subgraph cluster_vnet1 {
        label="VNet-East (10.0.0.0/16)";
        style="rounded,filled";
        color="#0078D455";
        bgcolor="#E5F1FB";

        VM1 [label="VM1\n(10.0.1.4)"];
    }

    // VNet West
    subgraph cluster_vnet2 {
        label="VNet-West (10.1.0.0/16)";
        style="rounded,filled";
        color="#0078D455";
        bgcolor="#E5F1FB";

        VM2 [label="VM2\n(10.1.1.4)"];
    }

    // VNet Peering
    VM1 -> VM2 [
        label="VNet Peering\nAllowGatewayTransit: Yes",
        style=dotted,
        dir=both,
        color="#0078D4"
    ];
}
```

Pro Tip: Use `constraint=false` to prevent layout distortion:  

```bash
Frontend -> Backend [constraint=false];
```

## 6. Troubleshooting Subgraphs  

| Issue | Solution |  
|-------|----------|  
| Border missing | Ensure `cluster_` prefix is used |  
| Label cutoff | Increase `labelloc` or reduce font size |  
| Overlap | Add `compound=true` to graph |  
| Arrow gaps | Use `lhead`/`ltail` attributes |  

## 7. Advanced Techniques  

### Invisible Subgraphs for Layout  

```dot
digraph InvisibleGrouping {
    subgraph {
        rank="same"; // Force alignment
        A; B; C [style=invis];
    }
    A -> B -> C [style=invis];
}
```

### Conditional Display  

```dot
digraph Conditional {
    subgraph cluster_prod {
        label="Production";
        App1; App2;
        
        // Only shown when detailed=true
        { rank=same; LB; Monitor; }
    }
}
```
