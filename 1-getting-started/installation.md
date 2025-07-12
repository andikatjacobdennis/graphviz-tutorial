# Graphviz Installation Guide for Windows 11

*For Azure Architecture Diagrams*  

## Method 1: Standard Installation (Recommended)

### Step 1: Download Graphviz

1. Visit the official Graphviz download page:  
   → [https://graphviz.org/download/](https://graphviz.org/download/)  
2. Under *Stable Windows installers*, click:  
   `graphviz-x.xx.xx.msi`  (latest version)  

### Step 2: Run the Installer

1. Double-click the downloaded `.msi` file.  
2. Follow the installer prompts:  
   - Install for all users  (recommended)  
   - Add Graphviz to PATH  (critical for command-line use)

3. Complete the installation.  

### Step 3: Verify Installation

1. Open Command Prompt  (`Win + R` → `cmd`).  
2. Run:  

   ```bash
   dot -V
   ```  

   Expected output:

   ```bash
   dot - graphviz version x.xx.xx
   ```  

## Method 2: Chocolatey (For Developers)

If you use Chocolatey (Windows package manager), run:  

```bash
choco install graphviz
```  

## Step 4: Set Up VS Code (Optional but Recommended)

1. Install [VS Code](https://code.visualstudio.com/download).  
2. Open Extensions (`Ctrl+Shift+X`) and install:  
   - Graphviz Preview (by EFanZh)
   - Markdown Preview Enhanced (by Yiyi Wang)

3. Create a `.dot` file and click on preview button on top right to preview diagrams.  

## Step 5: Test with an Azure Diagram

1. Create a file `azure-test.dot`:  

   ```dot
   digraph AzureDemo {
      rankdir="LR";
      node [shape=box3d, color=blue];
      
      User [shape=oval, label="Azure AD User"];
      WebApp [label="App Service"];
      SQL [shape=cylinder, label="Azure SQL DB"];
      
      User -> WebApp [label="HTTPS"];
      WebApp -> SQL [label="Connection String"];
   }
   ```  

2. Generate the diagram:  

   ```bash
   dot -Tpng azure-test.dot -o azure-test.png
   ```  

3. Open `azure-test.png` to see your first Azure diagram!

## Troubleshooting

| Issue | Solution |  
|-------|----------|  
| `dot` not recognized | Reboot or manually add `C:\Program Files\Graphviz\bin` to [PATH](https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/). |  
| VS Code preview not working | Ensure the Graphviz extension is active. |  
| Broken arrows/layouts | Update to the latest Graphviz version. |  
