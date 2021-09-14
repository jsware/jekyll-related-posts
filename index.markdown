---
layout: home
title: Jekyll Related Posts
---
Include a configurable set of related posts on a page that does not rely on custom plugins so works on GitHub Pages.

The core of `jekyll-related-posts` is an `_includes` file called `related.html`. This file first generates and then iterates a related posts list based on your site `_config.yml` and page front matter.

See [About]({{ site.baseurl}}{% link about.markdown %}) for further details and installation instructions

Or explore the demonstration below first...

This demonstration has site settings of:
```yaml
related_by: {{ site.related_by }}
related_limit: {{ site.related_limit }}
related_categories: {{ site.related_categories }}
related_tags: {{ site.related_tags }}

```