# Cisco Intersight Server Profile Template Automation

This Ansible playbook automates the creation and management of a basic Cisco Intersight Server Profile Template and derived Server Profiles.

## Features

- ✅ **Idempotent**: Safe to run multiple times
- ✅ **Smart Detection**: Automatically detects existing resources
- ✅ **Error Handling**: Robust error handling for API issues
- ✅ **Configurable**: Easy to customize for your environment
- ✅ **Debug Output**: Clear logging and status information

## Prerequisites

### 1. Install Required Software

```bash
# Install Ansible
pip3 install ansible

# Install Cisco Intersight Collection
ansible-galaxy collection install cisco.intersight

# Install Python dependencies
pip3 install intersight-auth requests

2. Cisco Intersight API Setup

    Log in to Cisco Intersight
    Go to Settings > API Keys
    Click Generate API Key
    Download the private key file
    Copy the API Key ID


3. Configure Credentials
    
    Copy the credentials template:
	cp intersight_credentials.yml.template intersight-credentials.yml
   
     Edit intersight_credentials.yml and add your:

    API Key ID
    Path to your private key file

Configuration

Edit the variables in intersight_server_profile_template.yml:

vars:
  template_name: YOUR_TEMPLATE_NAME              # Server Profile Template name
  org_name: default                              # Intersight organization
  imc_access_policy: YOUR_ACCESS_POLICY_NAME     # Your Access Policy
  ntp_policy: YOUR_NTP_POLICY_NAME               # Your NTP Policy  
  uuid_pool: YOUR_UUID_POOL_NAME                 # Your UUID Pool
  num_profiles: 4                                # Number of profiles to create

Usage

Basic usage:
# Run the playbook
ansible-playbook intersight_server_profile_template.yml

# Run with verbose output
ansible-playbook intersight_server_profile_template.yml -v

Advanced options

# Skip template creation (use existing template)
ansible-playbook intersight_server_profile_template.yml -e skip_template_creation=true

# Custom profile names (instead of numeric suffixes)
ansible-playbook intersight_server_profile_template.yml -e '{"profile_names": ["WebServer-01", "WebServer-02", "AppServer-01"]}'


What This Playbook Does

    Discovers Resources: Finds your organization, policies, and pools
    Template Management: Creates or uses existing Server Profile Template
    Profile Creation: Derives Server Profiles from the template
    Smart Logic: Only creates resources that don't already exist


File Structure

.
├── intersight_server_profile_template.yml    # Main playbook
├── derive_profiles.yml                       # Profile creation tasks
├── intersi_ht-credentials.yml.template       # Credentials template
└── README.md                                # This file

Common Issues

    Module not found error:
	ansible-galaxy collection install cisco.intersight --force

    API authentication errors:
        Verify your API Key ID and private key file
        Check file permissions on the private key

    Policy/Pool not found:
        Verify the policy/pool names in your variables
        Check that resources exist in Intersight


Debug Mode

Run with maximum verbosity to see detailed API calls:
	ansible-playbook intersight_server_profile_template.yml -vvv

Security Notes

    Never commit intersight_credentials.yml to version control
    Store private keys securely with appropriate file permissions
    Use Ansible Vault for additional security if needed:
	ansible-vault encrypt intersight-credentials.yml
