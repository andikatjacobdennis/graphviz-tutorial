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
    rankdir=LR;
    node [style=filled, fillcolor=lightgray, fontsize=10];

    // Basic Shapes
    box        [label="box", shape=box];
    ellipse    [label="ellipse", shape=ellipse];
    oval       [label="oval", shape=oval];
    circle     [label="circle", shape=circle];
    plaintext  [label="plaintext", shape=plaintext];
    diamond    [label="diamond", shape=diamond];
    triangle   [label="triangle", shape=triangle];
    trapezium  [label="trapezium", shape=trapezium];
    parallelogram [label="parallelogram", shape=parallelogram];
    house      [label="house", shape=house];
    hexagon    [label="hexagon", shape=hexagon];
    octagon    [label="octagon", shape=octagon];
    doubleoctagon [label="doubleoctagon", shape=doubleoctagon];
    egg        [label="egg", shape=egg];
    note       [label="note", shape=note];
    tab        [label="tab", shape=tab];
    folder     [label="folder", shape=folder];
    box3d      [label="box3d", shape=box3d];
    component  [label="component", shape=component];
    Mrecord    [label="Mrecord", shape=Mrecord];  // legacy, prefer 'record'

    // Database and Technology
    cylinder   [label="cylinder", shape=cylinder];
    invtrapezium [label="invtrapezium", shape=invtrapezium];
    invtriangle [label="invtriangle", shape=invtriangle];

    // Records and complex layout
    record     [label="record", shape=record];

    // Circular group
    subgraph cluster_circles {
        label="Circle-based"
        style=dashed
        circle;
        oval;
        egg;
    }

    // Box group
    subgraph cluster_boxes {
        label="Box-based"
        style=dashed
        box;
        box3d;
        folder;
        component;
        note;
        tab;
        record;
    }
}

```  

### Edges (Connections)  

```dot
digraph Connections {
    //rankdir=LR;
    node [shape=ellipse, style=filled, fillcolor="#e6f2ff"];
    
    // Standard edge
    A -> B [label="Standard"];
    
    // Colored edge
    A -> C [label="Red", color=red];

    // Bold edge
    A -> D [label="Bold", style=bold];

    // Dashed edge
    A -> E [label="Dashed", style=dashed];

    // Dotted edge
    A -> F [label="Dotted", style=dotted];

    // Thick edge
    A -> G [label="Thick", penwidth=3];

    // Invisible edge
    A -> H [label="Invisible", style=invis];

    // Arrowhead variations
    A -> I [label="Arrow: normal", arrowhead=normal];
    A -> J [label="Arrow: vee", arrowhead=vee];
    A -> K [label="Arrow: diamond", arrowhead=diamond];
    A -> L [label="Arrow: box", arrowhead=box];
    A -> M [label="Arrow: crow", arrowhead=crow];
    A -> N [label="Arrow: tee", arrowhead=tee];
    A -> O [label="Arrow: dot", arrowhead=dot];
    A -> P [label="Arrow: inv", arrowhead=inv];

    // Double arrows (both directions)
    A -> Q [label="Both", dir=both, arrowhead=normal, arrowtail=vee];

    // No arrowhead
    A -> R [label="None", arrowhead=none];

    // Tailed edge
    A -> S [label="Tail Arrow", dir=back, arrowtail=diamond];

    // Colored arrowhead
    A -> T [label="Colored Arrow", color=blue, fontcolor=blue, arrowhead=vee];

    // Curved edge with constraint
    A -> U [label="Constraint=false", constraint=false, style=dashed];
}

```

## 3. Common Errors & Fixes  

| Problem | Solution |  
|---------|----------|  
| Overlapping nodes | Add `splines=true;` to diagram |  
| Misaligned arrows | Use `rank=same;` for peer nodes |  
| Text cutoff | Increase `margin` attribute |  
