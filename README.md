# 🔐 Zero Trust Security Automation on GCP with n8n & MCP
An advanced Zero Trust architecture implementation on Google Cloud Platform, leveraging IAP, Access Context Manager, VPC Service Controls, and automating complex workflows with n8n powered by MCP + LLM decisioning.

## 📖 Overview
This project implements a comprehensive Zero Trust security model on GCP by integrating core GCP services with n8n for workflow automation and a custom Model Context Protocol (MCP) server for intelligent, LLM-driven decision-making.

- Enforces strong identity and context-aware access using IAP and Access Context Manager.
- Creates data perimeters with VPC Service Controls.
- Automates access requests, policy audits, and suspicious activity response with n8n.
- Leverages MCP + LLM for dynamic, intelligent decisioning within n8n workflows.

## 🚀 Features

- Identity-Aware Proxy (IAP) integration: Secures web applications by verifying user identity and context.
- Access Context Manager (ACM): Defines granular access levels based on device, IP, region, etc.
- VPC Service Controls (VPC SC): Creates secure perimeters around sensitive data and services.
- Automated workflows with n8n: Streamlines processes for access requests, policy audits, and incident response.
- MCP Server for LLM-powered decisioning: Integrates Large Language Models (LLMs) into n8n workflows for advanced policy evaluation and authorization.
- Infrastructure as Code with Terraform: Ensures consistent and repeatable deployment of all GCP resources.
- Secret Management with Secret Manager: Securely stores API keys and credentials for n8n and MCP.

## 💡 Architecture

### 📊 Architecture Diagram
(docs/architecture.png)

### Explanation of the Above Flow ☝️:
Here-
- User Access Attempt: A user attempts to access a protected application (e.g., hosted on Cloud Run or App Engine).
- Identity-Aware Proxy (IAP): IAP intercepts the request, verifies the user's identity, and checks their IAP access policy.
- Access Context Manager (ACM): IAP evaluates if the user's current context (IP, device, region, etc.) meets the defined access levels.
- VPC Service Controls (VPC SC): If the application or accessed data is within a VPC SC perimeter, the perimeter checks are enforced, preventing unauthorized data exfiltration or access.

### n8n Workflow Automation:
- Access Request: A user submits an access request (e.g., via a simple web form triggering an n8n webhook).
- MCP Server (Cloud Run): The n8n workflow sends the access request details to the MCP server.
- LLM (OpenAI/Vertex AI): The MCP server interacts with the LLM to evaluate the request based on context, user role, and policy, providing a recommendation.
- MCP Decision: The MCP server processes the LLM's response, applies logic, and returns a clear decision (e.g., "approve with temporary role", "deny").
- IAM API: If approved, n8n, driven by the MCP decision, interacts with the Google IAM API to grant temporary access.
- Suspicious Activity: Logs from IAP, ACM, VPC SC, and other services are routed to Pub/Sub. n8n subscribes to these topics and, using MCP/LLM for analysis, triggers alerts (e.g., to email or a way of contact which we will provide)
- Cloud Monitoring & Logging: logging and monitoring for all access attempts, policy violations, and automation outcomes will be recorded.

## 🛠️ Prerequisites
- GCP project/project ID.
- Service account with Logging Viewer + Functions Invoker roles
- Cloud Identity / Google Workspace should be configured.
- Service Accounts with minimal permissions for n8n, MCP server, and automation tasks.
- OpenAI API Key or Vertex AI (Google Cloud) setup for LLM access.
