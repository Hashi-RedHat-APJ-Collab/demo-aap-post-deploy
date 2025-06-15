# ServiceNow Developer Instance Provisioning

This playbook automates the provisioning of ServiceNow developer instances using the ServiceNow Developer Portal API.

## Requirements

- Red Hat Ansible Automation Platform (AAP)
- ServiceNow Developer Program account
- Network access to ServiceNow Developer Portal

## Playbook Overview

The `provision-servicenow-dev.yml` playbook performs the following tasks:

1. Authenticates with ServiceNow Developer Portal
2. Requests a new developer instance
3. Polls for completion status
4. Validates instance accessibility
5. Returns instance information

## Variables

### Required Variables

- `servicenow_dev_username`: ServiceNow Developer Portal username
- `servicenow_dev_password`: ServiceNow Developer Portal password

### Optional Variables

- `instance_name`: Desired instance name (default: auto-generated using current timestamp)
- `poll_interval`: Time between status checks in seconds (default: 60)
- `timeout`: Maximum wait time in seconds (default: 1800 / 30 minutes) - Note: Internally renamed to `max_timeout` to avoid Ansible reserved names

## AAP Job Template Configuration

### Basic Settings

- **Name**: ServiceNow Developer Instance Provisioning
- **Description**: Automates the provisioning of ServiceNow developer instances
- **Job Type**: Run
- **Inventory**: localhost
- **Project**: [Your Project Name]
- **Playbook**: playbooks/service_now/provision-servicenow-dev.yml
- **Credentials**: Machine credential (for localhost)

### Survey Configuration

Create a survey with the following fields:

1. **ServiceNow Developer Username**
   - Variable name: `servicenow_dev_username`
   - Type: Text
   - Question prompt: "ServiceNow Developer Portal Username"
   - Default answer: [leave empty]
   - Required: Yes
   - Make variable sensitive: No

2. **ServiceNow Developer Password**
   - Variable name: `servicenow_dev_password`
   - Type: Password
   - Question prompt: "ServiceNow Developer Portal Password"
   - Default answer: [leave empty]
   - Required: Yes
   - Make variable sensitive: Yes

3. **Instance Name (Optional)**
   - Variable name: `instance_name`
   - Type: Text
   - Question prompt: "Desired Instance Name (optional)"
   - Default answer: [leave empty]
   - Required: No
   - Make variable sensitive: No
   - Description: "If left blank, a unique name will be auto-generated"

4. **Poll Interval**
   - Variable name: `poll_interval`
   - Type: Integer
   - Question prompt: "Poll Interval in seconds"
   - Default answer: 60
   - Required: No
   - Minimum value: 10
   - Maximum value: 300
   - Make variable sensitive: No
   - Description: "Time between status checks (in seconds)"

5. **Maximum Timeout**
   - Variable name: `timeout`
   - Type: Integer
   - Question prompt: "Maximum Timeout in seconds"
   - Default answer: 1800
   - Required: No
   - Minimum value: 300
   - Maximum value: 3600
   - Make variable sensitive: No
   - Description: "Maximum wait time before giving up (in seconds)"

### Extra Variables

You can also set default values using Extra Variables:

```yaml
servicenow_dev_username: ""
servicenow_dev_password: ""
instance_name: ""
poll_interval: 60
timeout: 1800
```

## Execution Environment

Use an execution environment with the following packages:

- ansible.builtin
- community.general

## Troubleshooting

### Common Issues

1. **Authentication Failure**
   - Verify ServiceNow Developer Portal credentials
   - Check for special characters in password that may need escaping

2. **Instance Provisioning Timeout**
   - ServiceNow Developer Portal may be experiencing high demand
   - Increase the timeout value and retry

3. **Instance Provisioning Error**
   - Check the ServiceNow Developer Portal for quota limitations
   - You may have reached the maximum number of allowed instances

4. **Network Connectivity Issues**
   - Ensure the Ansible controller has network access to developer.servicenow.com

## Notes

- ServiceNow developer instances have limited availability
- Instances automatically hibernate after a period of inactivity
- The API may have rate limiting considerations
