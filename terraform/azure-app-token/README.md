# Azure App Token Action

This GitHub Action authenticates with Azure using OIDC and retrieves an access token from an Entra ID enterprise application.

## Usage

```yaml
- name: Get Azure App Token
  uses: ./.github/actions/terraform/azure-app-token
  with:
    arm_client_id: ${{ secrets.AZURE_CLIENT_ID }}
    arm_tenant_id: ${{ secrets.AZURE_TENANT_ID }}
    app_resource_id: "00000000-0000-0000-0000-000000000000"
```

The token is automatically exported as `TF_VAR_app_access_token` for use in Terraform configurations.

## Inputs

- `arm_client_id`: (Required) The client ID of your Azure AD application
- `arm_tenant_id`: (Optional) The Azure AD tenant ID
- `app_resource_id`: (Required) The resource ID of the Entra ID enterprise application