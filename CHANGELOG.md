## Netlify Deploy Pre-rendered Site v1.0.0

Initial release.

Example usage:

```yaml
- name: Deploy to Netlify 🚀
  uses: data-intuitive/netlify-deploy-action@v1
  with:
    auth: ${{ secrets.NETLIFY_AUTH_TOKEN }}
    site: 'my-netlify-site'
    dir: '_site'
    prod: true
    message: 'Deploy production ${{ github.ref }}'
```

See the README for more information.