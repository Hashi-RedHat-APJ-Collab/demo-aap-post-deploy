# ServiceNow Dev Instance Provisioning - Ansible Job Template

## Objective
Create a single Ansible Automation Platform job template to provision a ServiceNow developer instance.

## Context
You need to automate the provisioning of ServiceNow developer instances using Red Hat Ansible Automation Platform. This should be a simple, single job template that handles requesting and waiting for a new developer instance to be available.

## Requirements

### Infrastructure
- Red Hat Ansible Automation Platform (AAP)
- ServiceNow Developer Program account
- Network access to ServiceNow Developer Portal

### Functionality
1. **Request Instance**: Use ServiceNow Developer Portal API to request a new developer instance
2. **Monitor Status**: Poll the API to check provisioning status
3. **Validate Access**: Confirm the instance is accessible and ready
4. **Return Details**: Provide instance URL and credentials

## Technical Specifications

### Required Variables
- `servicenow_dev_username`: ServiceNow Developer Portal username
- `servicenow_dev_password`: ServiceNow Developer Portal password
- `instance_name`: Desired instance name (optional)
- `poll_interval`: Time between status checks (default: 60 seconds)
- `timeout`: Maximum wait time (default: 30 minutes) - Note: Internally renamed to avoid Ansible reserved names

### API Endpoints
- ServiceNow Developer Portal API for instance requests
- Status check endpoints for monitoring provisioning

### Expected Outputs
- Instance URL
- Admin credentials
- Instance details (version, apps installed, etc.)
- Provisioning time and status

## Deliverables

### Single Playbook
Create one playbook (`provision-servicenow-dev.yml`) that:
- Authenticates with ServiceNow Developer Portal
- Requests new developer instance
- Polls for completion status
- Validates instance accessibility
- Returns instance information

### Job Template Configuration
Provide AAP job template settings:
- Playbook path
- Inventory configuration
- Credential types needed
- Variable definitions (configured as job template survey)
- Execution environment

### Basic Documentation
Include:
- Setup instructions
- Variable configuration (as job template survey)
- Troubleshooting common issues
- Sample job template configuration

## Success Criteria

- Single job template that provisions ServiceNow dev instance
- Automated status monitoring until ready
- Clear output with instance details
- Error handling for common failures
- Execution time under 30 minutes

## Constraints

- ServiceNow developer instances have limited availability
- API rate limiting considerations
- Instance auto-hibernation after period of inactivity

Please provide a simple, single-file Ansible playbook and job template configuration for ServiceNow developer instance provisioning only.