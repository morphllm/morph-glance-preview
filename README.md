# Glance - AI Browser Testing for Preview Deployments

Catch bugs before they hit production. Glance automatically tests every preview deployment with AI-powered browser testing.

https://github.com/user-attachments/assets/demo.mp4

## Get Started in 2 Minutes

### 1. Install the GitHub App

[**Install Glance →**](https://www.morphllm.com/dashboard/integrations/github)

### 2. Add to Your Workflow

```yaml
- uses: morphllm/glance-preview@v1
  with:
    api-key: ${{ secrets.MORPH_API_KEY }}
    preview-url: ${{ steps.deploy.outputs.url }}
```

That's it. Glance will test your preview and comment the results directly on your PR.

## Why Glance?

- **Zero test maintenance** - AI adapts to UI changes automatically
- **Real browser testing** - Not screenshots, actual interaction
- **Works with any stack** - Vercel, Netlify, custom infrastructure, self-hosted
- **PR comments** - Results posted where your team already works

## Full Example

```yaml
name: Preview Test
on: pull_request

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Deploy Preview
        id: deploy
        run: |
          # Your deployment script
          echo "url=https://pr-${{ github.event.pull_request.number }}.preview.example.com" >> $GITHUB_OUTPUT

      - name: Test with Glance
        uses: morphllm/glance-preview@v1
        with:
          api-key: ${{ secrets.MORPH_API_KEY }}
          preview-url: ${{ steps.deploy.outputs.url }}
          instructions: |
            Test the checkout flow:
            - Add item to cart
            - Complete purchase
            - Verify confirmation
```

## Inputs

| Input | Required | Description |
|-------|----------|-------------|
| `api-key` | Yes | Your Morph API key |
| `preview-url` | Yes | Preview deployment URL |
| `instructions` | No | Custom testing focus |

## Outputs

| Output | Description |
|--------|-------------|
| `test-id` | Test run ID |
| `status` | Test status |

---

[Get your API key →](https://www.morphllm.com/dashboard/integrations/github)
