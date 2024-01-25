+++
title = 'Fs'
date = 2024-01-21T08:53:40-05:00
draft = true
+++

https://ascii-tree-generator.com/

```
my-app/
├─ node_modules/
├─ public/
│  ├─ favicon.ico
│  ├─ index.html
│  └─ robots.txt
├─ src/
│  ├─ index.css
│  └─ index.js
├─ .gitignore
├─ package.json
└─ README.md
```

tree

```
myapp
├───.github
│   └───workflows
├───archetypes
├───assets
├───content
│   └───notes
│       └───abstracting-complexity
├───data
├───i18n
├───layouts
├───resources
│   └───_gen
│       └───assets
│           └───css
│               └───ananke
│                   └───css
├───static
└───themes
    └───papermod
        ├───.github
        │   ├───ISSUE_TEMPLATE
        │   └───workflows
        ├───assets
        │   ├───css
        │   │   ├───common
        │   │   ├───core
        │   │   ├───extended
        │   │   └───includes
        │   └───js
        ├───i18n
        ├───images
        └───layouts
            ├───partials
            │   └───templates
            ├───shortcodes
            └───_default
                └───_markup
```

tree /f

```
site
│   .gitignore
│   .gitmodules
│   .hugo_build.lock
│   hugo.toml
│   README.md
│
├───.github
│   └───workflows
│           main.yml
│
├───archetypes
│       default.md
│
├───assets
│       avatar.webp
│
├───content
│   │   archives.md
│   │   search.md
│   │
│   └───notes
│       │   fs.md
│       │   math.md
│       │   mermaid.md
│       │
│       └───abstracting-complexity
│               infrastructure.md
│               intro.md
│
├───data
├───i18n
├───layouts
├───resources
│   └───_gen
│       └───assets
│           └───css
│               └───ananke
│                   └───css
│                           main.css_83735de7ca999e9c17f3419b41b93fdb.content
│                           main.css_83735de7ca999e9c17f3419b41b93fdb.json
│
├───static
└───themes
    └───papermod
        │   go.mod
        │   LICENSE
        │   README.md
        │   theme.toml
        │
        ├───.github
        │   │   PULL_REQUEST_TEMPLATE.md
        │   │   stale.yml
        │   │
        │   ├───ISSUE_TEMPLATE
        │   │       bug_report.md
        │   │       config.yml
        │   │       new-blank-issue.md
        │   │
        │   └───workflows
        │           gh-pages.yml
        │
        ├───assets
        │   ├───css
        │   │   ├───common
        │   │   │       404.css
        │   │   │       archive.css
        │   │   │       footer.css
        │   │   │       header.css
        │   │   │       main.css
        │   │   │       post-entry.css
        │   │   │       post-single.css
        │   │   │       profile-mode.css
        │   │   │       search.css
        │   │   │       terms.css
        │   │   │
        │   │   ├───core
        │   │   │       license.css
        │   │   │       reset.css
        │   │   │       theme-vars.css
        │   │   │       zmedia.css
        │   │   │
        │   │   ├───extended
        │   │   │       blank.css
        │   │   │
        │   │   └───includes
        │   │           chroma-mod.css
        │   │           chroma-styles.css
        │   │           scroll-bar.css
        │   │
        │   └───js
        │           fastsearch.js
        │           fuse.basic.min.js
        │           license.js
        │
        ├───i18n
        │       ar.yaml
        │       be.yaml
        │       bg.yaml
        │
        ├───images
        │       screenshot.png
        │       tn.png
        │
        └───layouts
            │   404.html
            │   robots.txt
            │
            ├───partials
            │   │   anchored_headings.html
            │   │   author.html
            │   │   breadcrumbs.html
            │   │   svg.html
            │   │   toc.html
            │   │   translation_list.html
            │   │
            │   └───templates
            │           opengraph.html
            │           schema_json.html
            │           twitter_cards.html
            │
            ├───shortcodes
            │       collapse.html
            │       figure.html
            │       inTextImg.html
            │       katex.html
            │       ltr.html
            │       rawhtml.html
            │       rtl.html
            │
            └───_default
                │   archives.html
                │   baseof.html
                │   index.json
                │   list.html
                │   rss.xml
                │   search.html
                │   single.html
                │   terms.html
                │
                └───_markup
                        render-codeblock-katex.html
                        render-codeblock-mermaid.html
                        render-image.html
```