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
digraph EdgeTypes {
    rankdir=LR;
    node [shape=ellipse, style=filled, fillcolor="#e6f2ff", fontsize=10];

    A1 -> B1 [label="Standard"];
    A2 -> B2 [label="Red", color=red];

    A3 -> B3 [label="Bold", style=bold];
    A4 -> B4 [label="Dashed", style=dashed];
    A5 -> B5 [label="Dotted", style=dotted];
    A6 -> B6 [label="Thick", penwidth=3];
    A7 -> B7 [label="Invisible", style=invis];

    A8  -> B8  [label="Arrow: normal", arrowhead=normal];
    A9  -> B9  [label="Arrow: vee", arrowhead=vee];
    A10 -> B10 [label="Arrow: diamond", arrowhead=diamond];
    A11 -> B11 [label="Arrow: box", arrowhead=box];
    A12 -> B12 [label="Arrow: crow", arrowhead=crow];
    A13 -> B13 [label="Arrow: tee", arrowhead=tee];
    A14 -> B14 [label="Arrow: dot", arrowhead=dot];
    A15 -> B15 [label="Arrow: inv", arrowhead=inv];

    A16 -> B16 [label="Both", dir=both, arrowhead=normal, arrowtail=vee];
    A17 -> B17 [label="None", arrowhead=none];
    A18 -> B18 [label="Tail Arrow", dir=back, arrowtail=diamond];
    A19 -> B19 [label="Colored Arrow", color=blue, fontcolor=blue, arrowhead=vee];
    A20 -> B20 [label="Constraint=false", constraint=false, style=dashed];
}
```

## 3. Common Errors & Fixes  

| Problem | Solution |  
|---------|----------|  
| Overlapping nodes | Add `splines=true;` to diagram |  
| Misaligned arrows | Use `rank=same;` for peer nodes |  
| Text cutoff | Increase `margin` attribute |  
