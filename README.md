# Morph Preview Test Action

AI-powered browser testing for preview deployments.

## Usage

```yaml
- uses: morphllm/preview-test-action@v1
  with:
    api-key: ${{ secrets.MORPH_API_KEY }}
    preview-url: ${{ steps.deploy.outputs.url }}
    instructions: Test the checkout flow  # Optional
```

## Setup

1. Install the Morph GitHub App at [morphllm.com/dashboard/integrations/github](https://morphllm.com/dashboard/integrations/github)
2. Add `MORPH_API_KEY` to your repository secrets

## Inputs

| Input | Required | Description |
|-------|----------|-------------|
| `api-key` | Yes | Your Morph API key |
| `preview-url` | Yes | Preview deployment URL |
| `instructions` | No | Custom testing instructions |

## Outputs

| Output | Description |
|--------|-------------|
| `test-id` | The ID of the triggered test |
| `status` | The status of the test (started) |

## Example: Custom Deployment

```yaml
name: Preview Test
on: pull_request

jobs:
  deploy-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Deploy to EKS
        id: deploy
        run: |
          # Your deployment script
          echo "url=https://pr-${{ github.event.pull_request.number }}.preview.example.com" >> $GITHUB_OUTPUT

      - name: Run Morph Preview Test
        uses: morphllm/preview-test-action@v1
        with:
          api-key: ${{ secrets.MORPH_API_KEY }}
          preview-url: ${{ steps.deploy.outputs.url }}
          instructions: |
            Test the main user flows:
            - Login functionality
            - Dashboard navigation
            - Form submissions
```

## How It Works

1. Your CI/CD deploys to your custom infrastructure (EKS, self-hosted, etc.)
2. This action sends the preview URL to Morph
3. Morph's AI tests the preview deployment
4. Results are posted directly to your PR as a comment

## Requirements

- Morph GitHub App must be installed on your repository
- Valid Morph API key with credits
