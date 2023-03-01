Netlify Deploy Pre-rendered Site
================

Deploy a pre-rendered site to Netlify.

## Usage

``` yaml
- name: Deploy to Netlify 🚀
  uses: data-intuitive/netlify-deploy-action@v1
  with:
    auth: ${{ secrets.NETLIFY_AUTH_TOKEN }}
    site: 'my-netlify-site'
    dir: '_site'
    prod: true
    message: 'Deploy production ${{ github.ref }}'
```

## Inputs

- `auth` (*required*): Netlify auth token to deploy with. Generate the
  auth token
  [here](https://app.netlify.com/user/applications#personal-access-tokens).

- `alias` (*optional*): Specifies the alias for deployment, the string
  at the beginning of the deploy subdomain (string). Useful for creating
  predictable deployment URLs. Maximum 37 characters.

- `dir` (*required*): Specify a folder to deploy (string).

- `prod` (*optional*): Whether the site should be deployed to production
  (boolean).

- `message` (*optional*): A short message to include in the deploy log
  (string).

- `site` (*required*): A site name or ID to deploy to (string). You can
  retrieve the API ID on your Site Settings.

- `timeout` (*optional*): Timeout to wait for deployment to finish
  (string).

## Outputs

- `site-name`: The name of the Netlify site associated with the
  deployment.
- `deploy-id`: A unique identifier assigned by Netlify to the
  deployment. It is used to track and manage the deployment, and can be
  used to retrieve additional information about the deployment from the
  Netlify API.
- `deploy-url`: The URL of the deployed site. It indicates the URL where
  the deployed application can be accessed by end-users.
- `logs`: The URL of the deployment logs. It provides detailed
  information about the deployment process, including any errors or
  warnings that occurred during the deployment.

## Example with Quarto

This example action will run `quarto render` on a project and then
publish the site on Netlify. Remove the part about Quarto if not
relevant or using a different builder.

``` yaml
on:
  push:
    branches: [ main, master ]
  pull_request:

name: Render project

jobs:
  build-deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Install Quarto
        uses: quarto-dev/quarto-actions/setup@v2
      
      - name: Render Quarto Project
        uses: quarto-dev/quarto-actions/render@v2

      - name: Deploy to Netlify 🚀
        if: github.event_name != 'pull_request'
        uses: data-intuitive/netlify-deploy-action@v1
        with:
          auth: ${{ secrets.NETLIFY_AUTH_TOKEN }}
          dir: '_site'
          site: 'my-netlify-site'
          prod: true
          message: 'Deploy production ${{ github.ref }}'

      - name: Deploy preview
        id: deploy_preview
        if: github.event_name == 'pull_request'
        uses: data-intuitive/netlify-deploy-action@v1
        with:
          auth: ${{ secrets.NETLIFY_AUTH_TOKEN }}
          dir: '_site'
          site: 'my-netlify-site'
          alias: "${{ env.BRANCH_NAME }}"
          message: 'Deploy prooduction ${{ github.ref }}'

      - uses: thollander/actions-comment-pull-request@v2
        if: github.event_name == 'pull_request'
        with:
          message: |
            [![Deploy: success](https://img.shields.io/badge/Deploy-success-success)](${{ steps.deploy_preview.outputs.deploy-url }})
          comment_tag: deploy_status

      - uses: thollander/actions-comment-pull-request@v2
        if: github.event_name == 'pull_request' && failure()
        with:
          message: |
            [![Deploy: failure](https://img.shields.io/badge/Deploy-failure-critical)]${{ steps.deploy_preview.outputs.logs }})
          comment_tag: deploy_status
```
