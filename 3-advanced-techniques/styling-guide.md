# Professional Styling Guide for Azure Diagrams  

## 1. Azure Brand Colors  

### Official Microsoft Palette  

```dot
digraph BrandColors {
    // Azure Blue
    AppService [fillcolor="#0078D4", style=filled, fontcolor=white];
    
    // Complementary Colors
    Storage [fillcolor="#0078D4", gradientangle=270, style="filled,radial"];
    SQL [fillcolor="#E5E5E5", color="#0078D4", style=filled];
    
    // Security Elements
    Firewall [fillcolor="#D83B01", style=filled, fontcolor=white];
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
    // Compute
    VM [shape=box3d, label="Azure VM"];
    AKS [shape=note, label="AKS Cluster"];
    
    // Data
    CosmosDB [shape=cylinder, label="Cosmos DB\n(SQL API)"];
    Blob [shape=folder, label="Blob Storage"];
    
    // Networking
    VNet [shape=box, style="rounded,filled", fillcolor="#F3F2F1"];
    LB [shape=pentagon];
    
    // Security
    KeyVault [shape=octagon, color="#D83B01"];
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
    // Standard connection
    App -> SQL [label="SQL 1433", fontsize=8];
    
    // Private link
    App -> PE [label="Private Endpoint", style=dotted, dir=both];
    
    // Blocked traffic
    Hacker -> SQL [label="Deny", color="#D83B01", style=dashed];
    
    // High volume
    Users -> App [label="HTTP/2", penwidth=2];
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
    // Global settings
    graph [nodesep=0.5, ranksep=1.2];
    
    // Force alignment
    {rank=same; WebApp; FunctionApp;}
    
    // Minimize edge crossings
    AppService -> {SQL, Redis, Blob} [constraint=false];
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
    // Global styles
    graph [fontname="Segoe UI"];
    node [fontname="Segoe UI", fontsize=10];
    edge [fontname="Segoe UI", fontsize=8];
    
    // Components
    User [shape=oval];
    WebApp [shape=box3d, fillcolor="#0078D4", style=filled, fontcolor=white];
    SQL [shape=cylinder, fillcolor="#E5E5E5", style=filled];
    
    // Connections
    User -> WebApp [label="HTTPS", color="#505050"];
    WebApp -> SQL [label="Encrypted", style=dashed];
    
    // Legend
    subgraph cluster_legend {
        label="Legend";
        style=dashed;
        
        Allow [shape=plaintext, label="Allow: Solid line"];
        Encrypt [shape=plaintext, label="Encrypted: Dashed line"];
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
