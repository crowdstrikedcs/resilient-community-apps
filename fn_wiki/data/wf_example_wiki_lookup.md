<!--
    DO NOT MANUALLY EDIT THIS FILE
    THIS FILE IS AUTOMATICALLY GENERATED WITH resilient-circuits codegen
-->

# Example: Wiki Lookup

## Function - Wiki Lookup

### API Name
`fn_wiki_lookup`

### Output Name
`None`

### Message Destination
`fn_wiki`

### Pre-Processing Script
```python
inputs.wiki_title = "VIPs"
inputs.wiki_search_term = artifact.value
```

### Post-Processing Script
```python
for wiki_result in results.matches:
  fields = wiki_result.split(',')
  dt = incident.addRow("vip_result")
  dt.first = fields[0]
  dt.last = fields[1]
  dt.location = fields[2]
  dt.email = fields[3]
  dt.role = fields[4]
```

---
