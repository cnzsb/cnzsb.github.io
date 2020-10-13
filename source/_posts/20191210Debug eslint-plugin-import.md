title: Debug eslint-plugin-import
date: 2019-12-10 16:26:24
tags: [Webpack, Eslint]

---

```bash
DEBUG=eslint-plugin-import:* $(npm bin)/eslint src/some_file_with_problems.js
```
