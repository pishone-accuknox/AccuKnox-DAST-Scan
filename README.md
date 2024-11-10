
# AccuKnox DAST Scan GitHub Action

## Learn More

- [About Accuknox](https://www.accuknox.com/)

**Description**  
This GitHub Action performs a Dynamic Application Security Testing (DAST) scan using OWASP ZAP, and then uploads the scan results to the AccuKnox CSPM panel. This action can be configured with specific inputs to seamlessly integrate with your DevSecOps pipeline.

## Usage

### Steps for Using AccuKnox DAST Scan Action in a Workflow YAML File

1. **Checkout into the Repo**  
   Use the checkout action to ensure your codebase is available for scanning.
   
2. **Add AccuKnox DAST Scan Action**  
   Use the `accuknox/dast-scan-action` repository with the desired version tag, e.g., `v1.0.0`.

3. **Token Generation from AccuKnox SaaS and Viewing Tenant ID**  
   To obtain the `accuknox_token` and `tenant_id` values needed to authenticate with AccuKnox:
   
   - **Navigate to Tokens**  
     Go to the **Settings** section in the AccuKnox SaaS sidebar.

     ![1](https://github.com/udit-uniyal/container-scan-action/assets/115368361/8f4e188b-d9f3-4404-83af-134d5dc1417a)
   
   - **Create Token**  
     In the "Tokens" section, click on **Create Token**. This action will display your `tenant_id` and allow you to generate an access token.

     ![2](https://github.com/udit-uniyal/container-scan-action/assets/115368361/296bc611-acb8-4918-9d6b-3a8ec7733377)
   
   - **Generate the Token**  
     After clicking **Generate**, copy the `accuknox_token` to use in the workflow.

   ![3](https://github.com/udit-uniyal/container-scan-action/assets/115368361/16032af0-bcac-4787-8f2a-a3fa0edc6ec6)

### Example Workflow File

```yaml
name: AccuKnox DAST Scan Workflow
on:
  push:
    branches:
      - main

jobs:
  dast-scan:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Run AccuKnox DAST Scan
        uses: accuknox/dast-scan-action@v1.0.0
        with:
          target_url: "http://testphp.vulnweb.com"
          accuknox_endpoint: ${{ secrets.ACCUKNOX_ENDPOINT }}
          tenant_id: ${{ secrets.TENANT_ID }}
          accuknox_token: ${{ secrets.ACCUKNOX_TOKEN }}
          label: "my-dast-scan"
```

## Input Values

| Input Value        | Description                                                | Optional/Required | Default Value |
|--------------------|------------------------------------------------------------|--------------------|---------------|
| `target_url`       | URL of the web application to be scanned.                  | Required          | None          |
| `accuknox_endpoint`| AccuKnox API endpoint URL to upload the scan results.      | Required          | None          |
| `tenant_id`        | Unique ID of the tenant for AccuKnox CSPM panel.           | Required          | None          |
| `accuknox_token`   | Token for authenticating with AccuKnox API.                | Required          | None          |
| `label`            | Label in AccuKnox SaaS for tagging scan results.           | Required          | None          |

## How it Works

- **OWASP ZAP DAST Scan**: The action runs a DAST scan on the specified target URL, using OWASP ZAP to identify security vulnerabilities in the application.
- **AccuKnox Report Generation**: The action generates a report in JSON format.
- **Report Upload**: The generated report is uploaded to the AccuKnox CSPM panel for centralized monitoring and insights.

## Notes

- Ensure all necessary secrets (`ACCUKNOX_ENDPOINT`, `TENANT_ID`, and `ACCUKNOX_TOKEN`) are securely stored in your repository's settings.
- AccuKnox panel provides a centralized view of all DAST results.
