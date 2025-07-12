# Your First Graphviz Diagrams

*From Basics to Azure Components*  

## 1. The "Hello World" of Graphviz

Create a file `hello.dot`:  

```dot
digraph HelloWorld {
    Start -> End;
}
```  

Generate the image:  

```bash
dot -Tpng hello.dot -o hello.png
```  

## 2. Basic Elements Explained  

### Nodes (Shapes)  

```dot
digraph Shapes {
    A [shape=box];       // Square
    B [shape=oval];      // Ellipse
    C [shape=cylinder];  // Database
    D [shape=box3d];     // Azure resource
}
```  

### Edges (Connections)  

```dot
digraph Connections {
    A -> B [label="HTTP"]; 
    A -> C [label="SQL" color="red"];
}
```

## 3. Your First Flow Diagram  

```dot
digraph UserFlow {
    rankdir="LR"; // Left-to-right layout
    
    User [shape=oval];
    Login [label="Login Page"];
    Dashboard [label="Dashboard"];
    
    User -> Login -> Dashboard;
    Login -> Error [label="Failed"];
    Error -> Login;
}
```  

## 4. Azure-Relevant Example  

### Basic Web App + Database  

```dot
digraph AzureBasic {
    rankdir="TB"; // Top-to-bottom
    
    // Components
    User [shape=oval];
    WebApp [shape=box3d, label="Azure App Service"];
    Database [shape=cylinder, label="Azure SQL"];
    
    // Connections
    User -> WebApp [label="HTTPS (443)"];
    WebApp -> Database [label="Connection String"];
}
```  

Key Features:  

- `box3d`: Standard shape for Azure resources  
- `cylinder`: Database representation  
- Explicit port labels (HTTPS)  

## 5. Layout Tips for Azure Diagrams  

### Grouping with Subgraphs  

```dot
digraph AzureWithVNet {
    subgraph cluster_vnet {
        label="Virtual Network";
        WebApp; Database;
    }
    User -> WebApp;
}
```  

### Direction Control  

```bash
# Command-line options
dot -Grankdir=LR -Tpng diagram.dot -o diagram.png 
```  

OR in-file:  

```dot
digraph {
    rankdir="LR"; // Alternatives: TB (top-bottom), RL (right-left)
}
```  

## 6. Common Errors & Fixes  

| Problem | Solution |  
|---------|----------|  
| Overlapping nodes | Add `splines=true;` to diagram |  
| Misaligned arrows | Use `rank=same;` for peer nodes |  
| Text cutoff | Increase `margin` attribute |  
