# Automating Azure Diagrams with Graphviz  

## 1. Basic Automation (PowerShell/Bash)  

### Batch Processing Script  

```powershell
# generate-diagrams.ps1
Get-ChildItem -Path .\azure\*.dot | ForEach-Object {
    $output = $_.FullName.Replace(".dot", ".png")
    dot -Tpng $_.FullName -o $output
    Write-Host "Generated: $output"
}
```

## 2. Dynamic Diagrams with Variables  

### Template Engine Approach  

1. Create `template.dot`:  

    ```bash
    digraph AzureTemplate {
        rankdir="{{ direction }}";
        {{! Nodes }}
        {{#components}}
        "{{ name }}" [shape={{ shape }}, label="{{ label }}"];
        {{/components}}
        {{! Connections }}
        {{#connections}}
        "{{ from }}" -> "{{ to }}" [label="{{ description }}"];
        {{/connections}}
    }
    ```

2. Use Python + Jinja2:  

    ```python
    # generate.py
    from jinja2 import Template
    import json

    with open('template.dot') as f:
        template = Template(f.read())

    with open('params.json') as f:
        data = json.load(f)

    output = template.render(data)
    with open('dynamic.dot', 'w') as f:
        f.write(output)
    ```

    Sample `params.json`:  

    ```json
    {
        "direction": "LR",
        "components": [
            {"name": "WebApp", "shape": "box3d", "label": "App Service"},
            {"name": "SQL", "shape": "cylinder", "label": "Azure SQL"}
        ],
        "connections": [
            {"from": "WebApp", "to": "SQL", "description": "Connection String"}
        ]
    }
    ```

## 3. CI/CD Integration  

### GitHub Actions Workflow  

```yaml
# .github/workflows/generate-diagrams.yml
name: Generate Diagrams
on: [push]

jobs:
  build:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install Graphviz
        run: choco install graphviz -y
      - name: Generate Diagrams
        run: |
          dot -Tpng ./architecture/main.dot -o ./docs/architecture.png
      - name: Upload Artifacts
        uses: actions/upload-artifact@v3
        with:
          name: diagrams
          path: ./docs/*.png
```

Key Features:  

- Installs Graphviz via Chocolatey  
- Processes `main.dot` on every push  
- Outputs diagrams as build artifacts  

## 4. Azure DevOps Pipeline  

```yaml
# azure-pipelines.yml
steps:
- script: |
    sudo apt-get install graphviz -y
    dot -Tsvg $(System.DefaultWorkingDirectory)/src/*.dot -o $(Build.ArtifactStagingDirectory)/diagrams/
  displayName: 'Generate SVG Diagrams'
  
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(Build.ArtifactStagingDirectory)/diagrams'
    ArtifactName: 'architecture-diagrams'
```

## 5. Real-World Use Cases  

### Auto-Generated Documentation  

Combine with MkDocs:  

```markdown
![Architecture Diagram](../diagrams/azure-arch.png)
<!-- Auto-updated on every merge to main -->
```

## Troubleshooting Automation  

| Issue | Solution |  
|-------|----------|  
| Missing Graphviz in CI | Add explicit installation step (`apt-get/choco`) |  
| Permission errors | Use `--install-actions=overwrite` in Chocolatey |  

Pro Tip: For large architectures:  

```bash
dot -Gnslimit=2 -Gmclimit=50 -Tsvg big-arch.dot -o optimized.svg
```
