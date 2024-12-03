---
{"dg-publish":true,"permalink":"/garden-notes/hosting-an-obsidian-digital-garden-on-git-hub-pages/","tags":["seedling","note"],"created":"2024-12-02T18:10","updated":"2024-12-03T17:28"}
---

# Hosting an Obsidian Digital Garden on GitHub Pages

**Key Steps for Deployment**:

- Deploy the digital garden in the same Git repository as a **new branch** and configure it as a **GitHub Page**.
- Use just one Git repository for simplicity.
- Rename the forked repository to `your_username.github.io` to match GitHub Pages' default setup.  
- Example:
    - Repo: [geschenkwald/geschenkwald.github.io](https://github.com/geschenkwald/geschenkwald.github.io)
    - Website: [geschenkwald.github.io](https://geschenkwald.github.io)
- Alternatively, configure a **custom domain** by modifying the `build.yaml` file to include a CNAME (e.g., `cognithink.de`).

**Enhancing with Comments**:

- Integrate [Giscus](https://giscus.app/) for a comment section.
- Modify the templates `src/site/_includes/layouts/index.njk` and `note.njk` to embed Giscus.  
- Example code snippet:

```html
<!-- ... -->
    {% endfor %}
{{ content | hideDataview | taggify | link | safe}}

<!-- comments section placeholder -->
<div class="giscus"></div>

<!-- giscus-script -->
<script src="https://giscus.app/client.js"
        data-repo="geschenkwald/geschenkwald.github.io"
        data-repo-id="R_kgDOLBDuTA"
        data-category="Announcements"
        data-category-id="DIC_kwDOLBDuTM4Cf6VU"
        data-mapping="title"
        data-strict="0"
        data-reactions-enabled="0"
        data-emit-metadata="0"
        data-input-position="top"
        data-theme="light"
        data-lang="de"
        data-loading="lazy"
        crossorigin="anonymous"
        async>
</script>

{% for imp in dynamics.common.afterContent %}
  <!-- ... -->
```

- Add yourself as a **collaborator** in the repository to manage comments directly without switching accounts.

**Automation Workflow**:

- Use a `build.yml` GitHub Actions workflow for deployment automation.
    - Triggers: Push or pull requests to the `main` branch.
    - Steps:
        1. Checkout repository.
        2. Use Node.js for building.
        3. Deploy to GitHub Pages with `peaceiris/actions-gh-pages`.
        4. Optionally set a custom domain with a CNAME.
    -  Example:
    
```yaml
name: GH Pages
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
jobs:
  build:
    runs-on: ubuntu-22.04
    strategy:
      matrix:        node-version: [20.x]
        # See supported Node.js release schedule at https://nodejs.org/en/about/releases/
    steps:
      - uses: actions/checkout@v3
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'
      - run: npm install
      - run: npm run build --if-present
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          publish_dir: ./dist
          github_token: ${{ secrets.GH_TOKEN }}
          cname: cognithink.de
```

---
- https://github.com/oleeskild/obsidian-digital-garden/discussions/160
- [https://github.com/geschenkwald/geschenkwald.github.io](https://github.com/geschenkwald/geschenkwald.github.io)