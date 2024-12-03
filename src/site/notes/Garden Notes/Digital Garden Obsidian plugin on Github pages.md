---
{"dg-publish":true,"permalink":"/garden-notes/digital-garden-obsidian-plugin-on-github-pages/","tags":["seedling","note"],"created":"2024-12-02T18:10","updated":"2024-12-03T15:35"}
---

# Digital Garden Obsidian plugin on Github pages

https://github.com/oleeskild/obsidian-digital-garden/discussions/160

I think the most convenient way is to deploy your digital garden in the very same git repo, but as a new branch and as a github page. this way you dont need netlify, vercel, a proxy server, docker or anything else. just one git repo and nothing more.  
you have to name the forked repo `your_user_name.github.io`. here is an example where i did exactly this (with my other github account):

[https://github.com/geschenkwald/geschenkwald.github.io](https://github.com/geschenkwald/geschenkwald.github.io)

so the belongig digital-garden is (by github design, as mentioned, username.github.io): [geschenkwald.github.io/](https://geschenkwald.github.io/)

or, as you can see in my modified build.yaml file, you can set a custom CNAME, which I set to: [cognithink.de](https://cognithink.de)

Additionally what is really really cool, you can integrate [giscus](https://giscus.app/) in your blog/digital garden by utilizing the discussion function of the same repo as well. to get this working you have to edit the template files slightly (which are `src/site/_includes/layouts/index.njk` and `note.njk`). so in my case for example i basically added the following:

```
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

And as you can see there is now a perfectly working comments sections below each of my blog articles and since i added myself (this account) as a collaborator in the settings of the geschenkwald.github.io repo, i can directly moderate, edit, delete comments in the discussion section without even having to logout and login with the other account.

[@oleeskild](https://github.com/oleeskild) if you are interested in this approach, i could make a new branch with a clean code and a pull request (or two, one for gh-page deployment and one for the giscus feature). additionally for giscus we could add an integration in the obsidian plugin, for example an option which the user can opt-in (like the file-tree-sidebar enable/disable etc).


## Workflow

https://github.com/geschenkwald/geschenkwald.github.io/blob/main/.github/workflows/build.yml

```
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
      matrix:
        node-version: [20.x]
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