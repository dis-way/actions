# Description

This GitHub Action is similar to `dis-way/actions/terraform/plan` but requires `dis-way/actions/terraform/init` to be run first. This allows you, for example, to run `terraform output` commands you might need before running the Plan.


## Sample
```yaml
jobs:
  plan:
    name: Plan
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4
      ...
      - name: Terraform Init
        uses: dis-way/actions/terraform/init@<hash>
        with:
          ...
      - name: Get something from Terraform output
        shell: bash
        run: |
          echo "SOMETHING=$(terraform output -raw something)" >> "$GITHUB_OUTPUT"
      ...
      - name: Terraform Plan
        uses: dis-way/actions/terraform/plan-only@<hash>
        with:
          working_directory: ${{ env.TF_PROJECT }}
          arm_client_id: ${{ env.ARM_CLIENT_ID }}
          arm_subscription_id: ${{ env.ARM_SUBSCRIPTION_ID }}
          tf_backend_state_name: ${{ env.TF_STATE_NAME }}
```