# Netlify Deploy Site v1.0.1

## Bug Fixes

* Fix a bug where `netlify deploy` included debug information in stdout, causing `parse error` in subsequent parsing of the outpus json.

# Netlify Deploy Site v1.0.0

Initial release.

Example usage:

```yaml
- name: Deploy to Netlify 🚀
  uses: data-intuitive/netlify-deploy-site@v1
  with:
    auth: ${{ secrets.NETLIFY_AUTH_TOKEN }}
    site: 'my-netlify-site'
    dir: '_site'
    prod: true
    message: 'Deploy production ${{ github.ref }}'
```

See the README for more information.
