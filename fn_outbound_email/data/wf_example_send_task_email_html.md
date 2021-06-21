<!--
    DO NOT MANUALLY EDIT THIS FILE
    THIS FILE IS AUTOMATICALLY GENERATED WITH resilient-circuits codegen
-->

# Example: Send Task Email Html

## Function - Outbound Email: Send Email

### API Name
`send_email`

### Output Name
`None`

### Message Destination
`email_outbound`

### Pre-Processing Script
```python
inputs.mail_to = rule.properties.mail_to
inputs.mail_cc = rule.properties.mail_cc
inputs.mail_attachments = rule.properties.mail_attachments
inputs.mail_incident_id = incident.id
inputs.mail_from = "changeme@resilientsystems.com"
inputs.mail_subject = u"[{0}] {1} Task:{2}".format(incident.id, incident.name, task.name)

from java.util import Date
creation_date = Date(incident.create_date)
type_ids = u", ".join(incident.incident_type_ids)
sev_code = u"{}".format(incident.severity_code)
current_plan = u"{}".format(incident.plan_status)

inputs.mail_body_html = u"""
<h2>Incident Summary</h2>
    Severity Code: {0}
<br>
    Plan Status: {1}
<br>
    Created: {2}
<br>
    Incident Type: {3}
<br>
    Task: {4}
<br>
    Instructions: 
<br>
{5}
""".format(sev_code, current_plan, creation_date, type_ids, task.name, task.instructions.get("content"))

```

### Post-Processing Script
```python
if results.success:
  noteText = u"Email Sent if mail server is valid/authenticated\n From: {0}\n To: {1}\n CC: {2}\n BCC: {3}\n Subject: {4}\n Body: {5}".format(results.content.inputs[0].strip("u\"[]"), results.content.inputs[1].strip("u\"[]"), results.content.inputs[2].strip("u\"[]"), results.content.inputs[3].strip("u\"[]"), results.content.inputs[4].strip("u\""), results.content.text )   
else:
  noteText = u"Email NOT Sent\n From: {0}\n To: {1}".format(results.content.inputs[0].strip("u\"[]"), results.content.inputs[1].strip("u\"[]"))
incident.addNote(noteText)
```

---
