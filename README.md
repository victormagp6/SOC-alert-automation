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

## Automation Workflow

First, I created a Splunk alert to detect failed Windows authentication attempts. The alert is triggered when the selected authentication event occurs more than two times, indicating possible brute-force activity. When the threshold is reached, Splunk sends the alert data to N8N through a webhook.

<img width="1598" height="270" alt="Screenshot from 2026-09-06 16-39-05" src="https://github.com/user-attachments/assets/7b99a3f6-bd1f-40e4-a255-27d36c8b321d" />

Next, I created an N8N workflow to process the alert received from Splunk. The workflow sends the alert data to an AI model, which generates a structured analysis.

<img width="1847" height="792" alt="Screenshot from 2026-09-05 18-05-14" src="https://github.com/user-attachments/assets/f0d4a6b7-8345-48d5-801c-fc9941fbe0ee" />

Finally, N8N sends the AI-generated analysis to a dedicated Slack channel. This allows the security team or analyst to receive the notifications in a central location, review the available evidence and recommendations and begin further investigation if necessary.

<img width="1801" height="927" alt="Screenshot from 2026-09-07 10-21-09" src="https://github.com/user-attachments/assets/188b80a0-18d9-4cf1-bdf2-8d9a74fa6d0d" />



## Investigation and findings

After receiving the Slack notification, I began the investigation in Splunk be reviewing the time range  in which the alert was triggered.

<img width="1801" height="927" alt="Screenshot from 2026-09-07 10-21-09" src="https://github.com/user-attachments/assets/8aac5378-c8a8-457a-81d9-5e4c2352ebb0" />
<img width="1748" height="743" alt="Screenshot from 2026-09-05 18-02-39" src="https://github.com/user-attachments/assets/4f474b26-2912-42c0-8fcb-d2bbf815776a" />
<img width="1748" height="743" alt="Screenshot from 2026-09-05 18-02-48" src="https://github.com/user-attachments/assets/b5df547e-ea9e-493c-8233-830d8a087d2b" />

The search identified three authentication-related events originating from the internal IP address '192.168.80.146'. The available logs showed failed authentication attempts followed by a successful logon. No additional suspicious activity, such as process execution or lateral movement was identified during the review period.

<img width="1323" height="804" alt="Screenshot from 2026-09-07 10-20-06" src="https://github.com/user-attachments/assets/aaeaf0ec-10c3-4d04-8e3b-f5eff556e639" />

I then verified the ownership of the source IP address and confirmed that '192.168.80.146' belonged to the internal IT department. After contacting the team, they confirmed that they had been performing authorized checks on company accounts.

The failed logon attempts occurred because the password for the affected account had recently been changed and the IT team was not aware of the update. Once the correct password was used, the authentication succeeded. 

Based on the available evidence and confirmation from the IT department, I closed the case and classified the alert as 'false positive' caused by an internal activity.

## Learning Outcomes

This project demonstrates practical experience with:

- SIEM alert configuration and Splunk detection logic.
- Windows authentication-event analysis.
- RDP failed-logon investigation.
- Webhook-based integrations.
- Security automation with n8n.
- Slack alerting workflows.
- AI-assisted alert triage.- SOC investigation methodology.
- Incident-response decision making.
- Secure handling of credentials and security telemetry.
