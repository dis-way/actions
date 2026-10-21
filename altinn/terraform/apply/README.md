# Introduction

This GitHub Action downloads the previously generated Terraform plan from GitHub artifacts and executes `terraform apply` with the downloaded plan. This action will not run unless the `dis-way/actions/altinn/terraform/plan` action has been executed first.

## Sample
```yaml
jobs:
  plan:
    name: Plan
    environment: prod
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Terraform Plan
        uses: dis-way/actions/altinn/terraform/plan@<hash>
        with:
          working_directory: ${{ env.TF_PROJECT }}
          oidc_type: environment
          oidc_value: ${{ env.ENVIRONMENT }}
          arm_client_id: ${{ env.ARM_CLIENT_ID }}
          arm_subscription_id: ${{ env.ARM_SUBSCRIPTION_ID }}
          tf_state_name: ${{ env.TF_STATE_NAME }}
          gh_token: ${{ secrets.GITHUB_TOKEN }}

  deploy:
    name: Deploy
    environment: prod
    needs: plan
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Terraform Apply
        uses: dis-way/actions/altinn/terraform/apply@<hash>
        with:
          working_directory: ${{ env.TF_PROJECT }}
          oidc_type: environment
          oidc_value: ${{ env.ENVIRONMENT }}
          arm_client_id: ${{ env.ARM_CLIENT_ID }}
          arm_subscription_id: ${{ env.ARM_SUBSCRIPTION_ID }}
          tf_state_name: ${{ env.TF_STATE_NAME }}

```