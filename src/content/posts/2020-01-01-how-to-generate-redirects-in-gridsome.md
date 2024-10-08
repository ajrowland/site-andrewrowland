---
title: How to generate redirects in Gridsome
date: 2020-01-01
published: true
comments: true
cover_image: ./images/gridsome.png
tags: ["JavaScript", "Gridsome"]
canonical_url: false
description: "I've switched this site from a hosted Wordpress solution, to a static site generated by Gridsome. Here's a brief word on creating redirects for the old post URLs."
---

I've switched this site from a hosted Wordpress solution, to a static site generated by [Gridsome](https://gridsome.org). Here's a brief word on creating redirects for the old post URLs.

This build uses the [@gridsome/source-filesystem](https://gridsome.org/plugins/@gridsome/source-filesystem) to import the content from a set of Markdown files. Each file exists as a page on this site, for example:

http://www.andrewrowland.com/how-to-generate-redirects-in-gridsome/

In the site's previous incarnation they would be found on:

http://www.andrewrowland.com/article/display/how-to-generate-redirects-in-gridsome/

I doubt anyone would have bookmarked any, but it would be nice to retain the old URLs, and redirect to the appropriate pages. Many hosts provide a mechanism to administer redirects. I'm using [Github Pages](https://pages.github.com), which doesn't if you're not using Jekyll. If you are, [this might be useful](https://github.com/jekyll/jekyll-redirect-from#redirect-to).

So for each post, we need to render an additional file for the old URL. This requires an amendment to the **gridsome.config.js** configuration. For a Post type this involves switching to an array of config objects. In this case, I've added an additional configuration for the redirect:

```javascript
module.exports = {
  templates: {
    Post: [
      {
        path: "/:title",
      },
      {
        name: "redirects",
        path: "/article/display/:title",
        component: "./src/templates/Redirect.vue",
      },
    ],
    Tag: "/tag/:id",
  },
};
```

If you look at the component property I've added a template for the rendered file. It's a minimal template designed to output the appropriate meta tag to perform the redirect:

```javascript
<template></template>

<script>
export default {
  metaInfo() {
    return {
        title: `Redirecting to ${this.$page.post.path}`,
        meta: [
            { 'http-equiv': 'refresh', content: `0; URL=${this.$page.post.path}`}
        ],
        link: [
            { rel: 'canonical', href: `http://www.andrewrowland.com${this.$page.post.path}` }
        ]
    }
  }
}
</script>

<page-query>
query Redirect ($id: ID!) {
  post: post (id: $id) {
    path
  }
}
</page-query>
```

Now, when we run a build, two files are generated for each post. One for the post itself, and one for the redirect.

I'm new to the framework, so if anyone knows of a better way to achieve this, please [let me know](mailto:mailATandrewrowlandDOTcom).

I guess I could do something on the [404](/404) page to check for the `/display/article` slug, and redirect. That would involve JavaScript, so this felt like a better solution.
