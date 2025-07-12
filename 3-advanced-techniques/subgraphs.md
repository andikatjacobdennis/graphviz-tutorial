# Mastering Subgraphs for Azure Diagrams  

## 1. Basic Subgraph Syntax  

### Virtual Network Container  

```dot
digraph VNetExample {
    subgraph cluster_vnet {
        label="Virtual Network (10.0.0.0/16)";
        bgcolor="lightgray";
        style="rounded,filled";
        
        VM1 [shape=box3d];
        VM2 [shape=box3d];
    }
    
    Internet -> VM1;
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
    subgraph cluster_rg_prod {
        label="RG-Production";
        AppService [label="App Service\n(Prod)"];
        SQL [label="Azure SQL\n(P1)"];
    }
    
    subgraph cluster_rg_dev {
        label="RG-Development"; 
        DevApp [label="App Service\n(Dev)"];
        DevSQL [label="Azure SQL\n(Basic)"];
    }
}
```

### Availability Zone Layout  

```dot
digraph ZonalArchitecture {
    subgraph cluster_zone1 {
        label="Zone 1";
        App1 [shape=box3d];
        Data1 [shape=cylinder];
    }
    subgraph cluster_zone2 {
        label="Zone 2";
        App2 [shape=box3d];
        Data2 [shape=cylinder];
    }
    
    LoadBalancer -> {App1, App2};
}
```

## 3. Nested Subgraphs  

### Hub-Spoke Network  

```dot
digraph HubSpoke {
    subgraph cluster_hub {
        label="Hub Network";
        Firewall [shape=doubleoctagon];
        
        subgraph cluster_shared {
            label="Shared Services";
            Monitor [shape=box3d];
            DNS [shape=box3d];
        }
    }
    
    subgraph cluster_spoke1 {
        label="Spoke 1";
        AppService;
    }
    
    Firewall -> AppService [style=dashed];
}
```

## 4. Cross-Subgraph Connections  

### Peering Relationships  

```dot
digraph VNetPeering {
    subgraph cluster_vnet1 {
        label="VNet-East";
        VM1;
    }
    
    subgraph cluster_vnet2 {
        label="VNet-West"; 
        VM2;
    }
    
    // Peering connection
    VM1 -> VM2 [
        label="Peering\nAllowGatewayTransit",
        style=dotted,
        dir=both
    ];
}
```

Pro Tip: Use `constraint=false` to prevent layout distortion:  

```dot
Frontend -> Backend [constraint=false];
```

## 5. Styling Subgraphs  

### Consistent Visual Language  

```dot
digraph StyledGroups {
    // Shared style for all clusters
    node [fontname="Segoe UI"];
    edge [fontsize=8];
    
    subgraph cluster_network {
        label="Network Layer";
        bgcolor="#F3F2F1";
        fontcolor="#0078D4";
        
        VNet [shape=box, style="rounded"];
        NSG [shape=note];
    }
    
    subgraph cluster_compute {
        label="Compute Layer";
        bgcolor="#E5F6FF";
        
        VM [shape=box3d];
        AKS [shape=note];
    }
}
```

## 6. Practical Azure Examples  

### Full Architecture with Subgraphs  

```dot
digraph FullAzureArch {
    // Global styles
    graph [fontname="Segoe UI"];
    node [fontsize=10];
    
    // Networking Layer
    subgraph cluster_network {
        label="Networking";
        
        subgraph cluster_hub {
            label="Hub";
            Firewall;
            ExpressRoute;
        }
        
        subgraph cluster_spoke1 {
            label="Spoke 1";
            AppService;
        }
    }
    
    // Data Layer
    subgraph cluster_data {
        label="Data";
        SQL;
        Storage;
    }
    
    // Connections
    AppService -> SQL;
    Firewall -> AppService [label="NSG Rules"];
}
```

## 7. Troubleshooting Subgraphs  

| Issue | Solution |  
|-------|----------|  
| Border missing | Ensure `cluster_` prefix is used |  
| Label cutoff | Increase `labelloc` or reduce font size |  
| Overlap | Add `compound=true` to graph |  
| Arrow gaps | Use `lhead`/`ltail` attributes |  

Example Fix:  

```dot
graph [compound=true];
App -> DB [lhead=cluster_db];
```

## 8. Advanced Techniques  

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
