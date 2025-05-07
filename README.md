# Acquia Environment Deployment Workflow

This repository contains a GitHub Actions workflow for automating the deployment and synchronization process between Acquia environments for a Drupal site.

## Workflow: Refresh Acquia mysitedev

The workflow in `.github/workflows/deploy.yml` automates the following tasks:

1. **Merges code** from master branch into mysitedev-main branch
2. **Synchronizes databases** between prod and mysitedev environments
3. **Exports and imports configurations** between environments
4. **Verifies Drupal site functionality** after changes

### Workflow Triggers

The workflow runs:
- Automatically once a day at midnight UTC
- Manually via GitHub Actions UI (workflow_dispatch)

### Required Secrets

The following secrets must be configured in your GitHub repository:

- `ACQUIA_API_KEY`: Your Acquia API key
- `ACQUIA_API_SECRET`: Your Acquia API secret
- `SSH_PRIVATE_KEY`: SSH private key for accessing Acquia repositories
- `SSH_KNOWN_HOSTS`: SSH known hosts for Acquia servers

### Workflow Steps

1. **Setup Environment**:
   - Installs DDEV
   - Configures SSH keys
   - Sets up Acquia API credentials

2. **Code Management**:
   - Clones Acquia repository
   - Merges master into mysitedev-main branch
   - Configures DDEV environment

3. **Database Operations**:
   - Pulls databases from mysitedev and prod environments
   - Exports configuration from mysitedev
   - Imports prod database for comparison

4. **Configuration Management**:
   - Compares configurations between environments
   - Identifies module differences
   - Exports combined configuration

5. **Verification and Deployment**:
   - Verifies Drupal site functionality
   - Commits configuration changes
   - Syncs database from prod to mysitedev
   - Imports configuration to mysitedev environment

6. **Cleanup**:
   - Stops DDEV
   - Removes sensitive files

## Usage

### Manual Trigger

1. Go to the "Actions" tab in your GitHub repository
2. Select the "Refresh Acquia mysitedev" workflow
3. Click "Run workflow"
4. Select the branch to run from (usually main)
5. Click "Run workflow"

### Setup Instructions

1. Fork or clone this repository
2. Configure the required secrets in your GitHub repository settings:
   ```bash
   # Example commands to set up secrets (using GitHub CLI)
   gh secret set ACQUIA_API_KEY --body="your-acquia-api-key"
   gh secret set ACQUIA_API_SECRET --body="your-acquia-api-secret"
   gh secret set SSH_PRIVATE_KEY --body="$(cat ~/.ssh/id_rsa)"
   gh secret set SSH_KNOWN_HOSTS --body="$(ssh-keyscan svn-xxxx.prod.hosting.acquia.com)"
   ```
3. Update the repository URL in the workflow file to match your Acquia repository
4. Ensure your Acquia environments are correctly named in the workflow file
5. Push changes to trigger the workflow

## Customization

You may need to customize the workflow for your specific environment:

1. Update the Acquia repository URL in the "Merge mysitedev-main with master" step
2. Modify environment names if they differ from "mysitedev" and "prod"
3. Adjust the configuration directory path if needed
4. Add or remove specific Drupal modules to check in the verification step

## Troubleshooting

If the workflow fails:
1. Check the workflow logs in GitHub Actions
2. Verify that all secrets are properly configured
3. Ensure Acquia API credentials have appropriate permissions
4. Check SSH key access to Acquia repositories
5. Verify DDEV configuration is correct for your environment

## Contributing

Contributions to improve the workflow are welcome. Please submit a pull request with your proposed changes.

## License

[MIT License](LICENSE)
