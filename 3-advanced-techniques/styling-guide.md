# Professional Styling Guide for Azure Diagrams  

## 1. Azure Brand Colors  

### Official Microsoft Palette  

```dot
digraph BrandColors {
    rankdir=LR;
    splines=ortho;

    // Global styling
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050", fontcolor="black"];

    // App Service - Azure Blue
    AppService [label="App Service", shape=box3d, style=filled, fillcolor="#0078D4", fontcolor=white];

    // Storage with Azure brand blue gradient
    Storage [label="Blob Storage", shape=folder, style="filled,radial", fillcolor="#0078D4", gradientangle=270, fontcolor=white];

    // SQL DB with official light fill and border
    SQL [label="Azure SQL DB", shape=cylinder, style=filled, fillcolor="#E5E5E5", color="#0078D4", fontcolor=black];

    // Azure Firewall - Security red
    Firewall [label="Azure Firewall", shape=doubleoctagon, style=filled, fillcolor="#D83B01", fontcolor=white];
}
```  

Color Reference:  

| Purpose | Hex Code |  
|---------|----------|  
| Primary Azure Blue | `#0078D4` |  
| Dark Contrast | `#106EBE` |  
| Alert/Deny | `#D83B01` (Orange) |  
| Allow/Success | `#107C10` (Green) |  

## 2. Node Styles by Component Type  

### Standardized Shapes  

```dot
digraph NodeStyles {
    rankdir=LR;
    splines=ortho;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050", fontcolor="black"];

    // Compute nodes
    VM  [label="Azure VM", shape=box3d, style=filled, fillcolor="#0078D4", fontcolor=white];
    AKS [label="AKS Cluster", shape=note, style=filled, fillcolor="#0078D4", fontcolor=white];

    // Data nodes
    CosmosDB [label="Cosmos DB\n(SQL API)", shape=cylinder, style=filled, fillcolor="#E5E5E5", color="#0078D4", fontcolor=black];
    Blob     [label="Blob Storage", shape=folder, style=filled, fillcolor="#E5E5E5", color="#0078D4", fontcolor=black];

    // Networking nodes
    VNet [label="Virtual Network", shape=box, style="rounded,filled", fillcolor="#F3F2F1", color="#0078D4", fontcolor=black];
    LB   [label="Load Balancer", shape=pentagon, style=filled, fillcolor="#F3F2F1", color="#0078D4", fontcolor=black];

    // Security node
    KeyVault [label="Key Vault", shape=octagon, style=filled, fillcolor="#FFD6D6", color="#D83B01", fontcolor=black];
}
```

Shape Convention:  

- `box3d`: All Azure resources (VMs, App Services)  
- `cylinder`: Databases (SQL, CosmosDB)  
- `folder`: Storage services  
- `pentagon`: Load balancers/Front Door  

## 3. Edge Styling for Clarity  

### Connection Types  

```dot
digraph ConnectionTypes {
    rankdir=LR;
    splines=ortho;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050", fontcolor="black"];

    // Nodes
    App     [label="App Service", shape=box3d, style=filled, fillcolor="#0078D4", fontcolor=white];
    SQL     [label="Azure SQL DB", shape=cylinder, style=filled, fillcolor="#E5E5E5", color="#0078D4"];
    PE      [label="Private Endpoint", shape=note, style=filled, fillcolor="#FFF4CE", color="#D83B01", fontcolor=black];
    Hacker  [label="Untrusted Source", shape=oval, style=dashed, color="#D83B01"];
    Users   [label="Users", shape=oval];

    // Edges
    App -> SQL     [label="SQL 1433"];
    App -> PE      [label="Private Endpoint", style=dotted, dir=both, color="#0078D4"];
    Hacker -> SQL  [label="Deny", color="#D83B01", style=dashed];
    Users -> App   [label="HTTP/2", penwidth=2];
}
```

Edge Rules:  

| Scenario | Style |  
|----------|-------|  
| Encrypted | `style=dashed` |  
| Private Link | `style=dotted` |  
| High bandwidth | `penwidth=2.5` |  
| Cross-region | `color="#8E8E8E"` (Gray) |  

## 4. Typography & Labels

### Readable Text Formatting  

```dot
digraph Typography {
    // Defaults for all nodes
    node [fontname="Segoe UI", fontsize=10];
    edge [fontname="Segoe UI", fontsize=8];
    
    // Multi-line labels
    AppGW [label="Application Gateway\n(WAF v2)\nTier: Standard"];
    
    // Status indicators
    Storage [label=<<table border="0">
        <tr><td><b>Blob Storage</b></td></tr>
        <tr><td>Performance: Premium</td></tr>
        <tr><td>Redundancy: ZRS</td></tr>
    </table>>];
}
```

Pro Tips:  

- Use `\n` for line breaks in simple labels  
- HTML-like (`<table>`) for complex labels  
- Bold primary resource names  

## 5. Layout Optimization  

### Professional Spacing  

```dot
digraph Layout {
    rankdir=TB;
    
    // Global settings
    graph [fontname="Segoe UI", fontsize=10, nodesep=0.5, ranksep=1.2];
    node  [fontname="Segoe UI", fontsize=9, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, penwidth=1.0, color="#505050"];

    // Node definitions with Azure-style shapes and colors
    WebApp      [label="Web App", shape=box3d, style=filled, fillcolor="#0078D4", fontcolor=white];
    FunctionApp [label="Function App", shape=box3d, style=filled, fillcolor="#0078D4", fontcolor=white];
    AppService  [label="App Service", shape=box3d, style=filled, fillcolor="#0078D4", fontcolor=white];
    SQL         [label="Azure SQL DB", shape=cylinder, style=filled, fillcolor="#E5E5E5", color="#0078D4"];
    Redis       [label="Azure Cache\nfor Redis", shape=cylinder, style=filled, fillcolor="#E5E5E5", color="#0078D4"];
    Blob        [label="Blob Storage", shape=folder, style=filled, fillcolor="#E5E5E5", color="#0078D4"];

    // Force alignment of components on the same horizontal level
    { rank = same; WebApp; FunctionApp }

    // Edge connections
    AppService -> {SQL Redis Blob} [constraint=false];
}
```

Layout Commands:  

```bash
# Command-line options
dot -Gnodesep=1.0 -Gsplines=ortho -Tpng input.dot -o output.png
```

## 6. Full Example: Styled Azure Architecture  

```dot
digraph StyledArch {
    rankdir=LR;

    // Global styles
    graph [fontname="Segoe UI", fontsize=10];
    node  [fontname="Segoe UI", fontsize=10, penwidth=1.2];
    edge  [fontname="Segoe UI", fontsize=8, color="#505050", penwidth=1.0];

    // Components
    User   [shape=oval, label="User"];
    WebApp [shape=box3d, label="App Service", style=filled, fillcolor="#0078D4", fontcolor="white"];
    SQL    [shape=cylinder, label="Azure SQL DB", style=filled, fillcolor="#E5E5E5", color="#0078D4", fontcolor="black"];

    // Connections
    User -> WebApp [label="HTTPS"];
    WebApp -> SQL [label="Encrypted", style=dashed];

    // Legend
    subgraph cluster_legend {
        label="Legend";
        style=dashed;
        fontsize=10;
        labelloc="t";
        fontname="Segoe UI";

        Allow   [shape=plaintext, label="▶ Solid line = Standard traffic"];
        Encrypt [shape=plaintext, label="▶ Dashed line = Encrypted traffic"];
    }
}
```

## 7. Troubleshooting Visual Issues  

| Problem | Solution |  
|---------|----------|  
| Cluttered nodes | Increase `nodesep` and `ranksep` |  
| Label overlap | Add `labelangle=15, labeldistance=2.5` |  
| Color bleed | Use `gradientangle=270` for gradients |  
| Arrow misalignment | Set `constraint=false` on problem edges |  

Pro Tip: For print-ready diagrams:  

```bash
dot -Tpdf -Gdpi=600 -Nfontsize=12 styled.dot -o print.pdf
```
