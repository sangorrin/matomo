# Strix Security Testing Workflow

## Overview

This workflow implements automated white-box security testing using Strix (https://github.com/usestrix/strix) to test Matomo's security posture, particularly focusing on protecting personal data.

## What This Workflow Does

1. **Deploys Matomo**: Sets up a complete Matomo instance with MySQL database
2. **Creates Test User**: Auto-generates credentials for a test user account
3. **Populates Analytics Data**: Injects tracking data containing personal information:
   - Names (e.g., John Smith, Jane Doe)
   - Phone numbers (e.g., +1-555-0123)
   - Email addresses (e.g., john.smith@example.com)
4. **Runs Strix Agents**: Executes Strix security testing agents that attempt to:
   - Hack the test user account
   - Steal personal data from analytics
   - Find authentication bypass vulnerabilities
   - Test authorization controls
   - Identify data exfiltration possibilities
5. **Saves Results**: All test results are saved as GitHub Actions artifacts

## Required GitHub Secrets

Configure these secrets in your repository settings (Settings → Secrets and variables → Actions):

- `STRIX_LLM`: The LLM provider to use (e.g., "openai", "anthropic")
- `LLM_API_KEY`: API key for the LLM provider

## Triggering the Workflow

### Automatic Execution
The workflow runs automatically every day at 2 AM UTC.

### Manual Execution
You can manually trigger the workflow:
1. Go to Actions tab in GitHub
2. Select "Strix Security Testing" workflow
3. Click "Run workflow"
4. Optionally specify test duration (default: 30 minutes)

## Reviewing Results

After the workflow completes:
1. Go to the workflow run page
2. Scroll to "Artifacts" section
3. Download:
   - `strix-security-test-results-{run_number}`: Contains Strix test results and configuration
   - `matomo-logs-{run_number}`: Contains Matomo application logs (if available)

## Test Configuration

The workflow performs white-box testing with:
- **Target**: Locally deployed Matomo instance
- **Source Code Access**: Full repository source code
- **Test Objectives**:
  - Attempt to access personal data (names, phones, emails)
  - Test account takeover vulnerabilities
  - Identify data exfiltration possibilities
  - Check for authentication bypass
  - Test authorization controls

## Personal Data Used in Testing

The workflow creates synthetic test data for 10 users, each with:
- Full name
- Phone number
- Email address

This data is sent as custom variables in Matomo tracking requests to simulate real-world analytics collection.

## Security Considerations

- All testing happens in an isolated GitHub Actions environment
- Test credentials are auto-generated and unique per run
- Sensitive data is cleaned up after workflow completion
- Artifacts are retained for 30 days for security review
- No production data or systems are accessed

## Customization

You can customize the test by modifying:
- Test duration via workflow dispatch input
- Personal data templates in the "Populate analytics with personal data" step
- Strix configuration in the "Configure Strix for white-box testing" step
- Test objectives and targets

## Troubleshooting

If the workflow fails:
1. Check the workflow run logs for error messages
2. Verify that required secrets are configured
3. Review the Strix configuration in artifacts
4. Check Matomo logs artifact for application errors

## Integration with CI/CD

This workflow is designed to:
- Run independently of other CI/CD pipelines
- Not block merges or deployments
- Provide security insights asynchronously
- Alert on discovered vulnerabilities through artifacts

## Next Steps

After implementing this workflow:
1. Configure required GitHub secrets
2. Run the workflow manually to verify setup
3. Review the first test results
4. Set up notifications for failed workflow runs
5. Integrate findings into your security review process
