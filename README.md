# SOC-alert-automation
The goal was to detect suspicious Windows authentication activity in Splunk, send the alert to N8N through a webhook, use AI to generate an initial security analysis, and publish the result in a Slack channel for analyst review.

The workflow simulates the security operation process:

```text
Windows Security Logs -> Splunk Detection -> N8N Webhook -> AI Analysis -> Slack Alert -> SOC Investigation
```

## Objectives

- Generate Windows authentication events in a controlled environment.
- Detect repeated failed logons with Splunk.
- Trigger a Splunk alert based on the detection logic.
- Send the alert to N8N through a webhook.
- Use AI to summarize and enrich the alert.
- Sena a structured notification to a Slack security-alert channel.
- Include MITRE ATT&CK context and recommended investigation actions.
- Practice SOC monitoring, alert triage and security automation.

## Lab Environment

| System | Operating System | Role | IP Address |
|---|---|---|---|
| Splunk Server | Ubuntu Server | SIEM, log collection, detection, and alerting | `192.168.80.141` |
| Windows 10 Endpoint | Windows 10 | Windows Security event source | `192.168.80.142` |
| n8n Server | Ubuntu Server | Workflow automation, AI analysis, and Slack integration | `192.168.80.143` |
| Windows 10 RDP Test Host | Windows 10 | Host used to generate failed RDP authentication attempts | `192.168.80.146` |

