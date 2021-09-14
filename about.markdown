---
layout: page
title: About
---
Include a configurable set of related posts on a page. `jekyll-related-posts' does not rely on custom plugins so works with GitHub Pages.

* Choose random pages from posts and collections.
* Choose pages with matching tags and/or categories.
* Change the number of related pages to include.
* Change the selection criteria (number of tags/categories, match both tags and categories or either).

The core of `jekyll-related-posts` is an `_includes` file called `related.html`. This file first generates and then iterates a related posts list based on your site `_config.yml` and page front matter.

## Installation

1. Head over to [jekyll-related-posts](https://github.com/jsware/jekyll-related-posts) and download the [`related.html`](https://github.com/jsware/jekyll-related-posts/blob/main/_includes/related.html) file.
1. Copy the `related.html` file into your site `_includes` directory.
1. Modify your `_config.yml` file to include `related_by`, `related_categories` and/or `related_tags` values. See below for available settings.
1. Update the `_layout` used to include related posts to {% raw %}`{% include related.html %}`{% endraw %} where related posts should appear.
1. Copy the `related-post.html` file into your _includes directory - modifying it to output a link for each related document. Alternatively set the `related_template` setting in `_config.yml` with an alternative include file - for example `archive-single.html` if you use the [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/) theme.

## Settings

The configured `related_template` file (defaults to `related-post.html`) is included by `related.html` for each post (as a `post` variable). Additionally, a `related_layout` value is provided in `include.type` to control how that related post is shown on your page. This is compatible with the popular Minimal Mistakes' [Grid View](https://mmistakes.github.io/minimal-mistakes/docs/layouts/#grid-view) layout in it's `archive-single` helper.

Configuration for `related.html` is controlled by a set of `related_*` settings. They can be set at the page level, which overrides a site-wide setting. Each setting has a default value if not set at page or site levels:

```
page.related_*? => Use page.related_*
 |
 No
 |
 \-- site.related_*? => Use site.related_*
   |
   No
   |
   \-- Use a default value.
```

The following `related_*` variables are available:

| Related Setting | Description |
|-----------------|-------------|
| `related_by`    | Space separated list defining how related posts are selected. Use **`tags`**, **`categories`** (or both separated by space to match tags and categories). Include the word **or** to match tags or categories.<br/><br/> Alternatively use **`random`** to choose a random set of pages, or **`default`** for `site.related_posts` (or `site.posts` if `site.related_posts` is empty). |
| `related_from` | Specify whether related pages come from just **`posts`**, the same **`collection`** or any **`documents`** (including posts).<br/><br/>NB: When using `related_by: random`, pages can only be selected from all `posts` or all `documents` (including posts). To select random pages from the same collection see the examples below. |
| `related_limit` | The maximum number of related pages to include. Defaults to **4**. |
| `related_categories` | The minimum **number** of matching categories when `related_by` contains `categories`. Defaults to **1**. |
| `related_tags` | The minimum **number** of matching tags when `related_by` contains `tags`. Defaults to **1**. |
| `related_template` | The name of the `_include` file used for each related post added to the page. Defaults to `related-post.html` For the [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/) theme, use `archive-single.html` instead. |
| `related_layout` | The layout to pass to the `related_template` in the `include.type` parameter. Can vary depending on the `related_template` used. Defaults to **`list`**. |

NB: Setting `related_categories` and `related_tags` to 0 when using `related_by` = `categories`, `tags` or both has a similar effect to configuring `related_by` as `random`.

## Example Configurations

Example configurations and integration of `related.html` into the `default` layout of the Jekyll `minima` theme is shown below:

**Include up to 6 pages with 1+ matching categories AND 1+ matching tags:**

```yaml
related_by: tags categories # Match on both categories and tags
related_limit: 6            # Include 6 related pages
related_categories: 1       # Match at least 1 category
related_tags: 1             # AND at least 1 tag.
```

**Include up to 4 pages with 1+ matching categories OR 1+ matching tags:**

```yaml
related_by: tags or categories # Match on categories OR tags
#related_limit: 4              # Include 4 related pages by default
related_categories: 1          # Match at least 1 category
related_tags: 2                # OR at least 2 tags.
```
*NB: `related_limit: 4` is the default value, so does not have to be specified.*

**Include up to 5 related pages with 2+ matching tags:**

```yaml
related_by: tags      # Match on both categories and tags
related_limit: 5      # Include 5 related pages
related_tags: 2       # Match with at least 2 tags.
```

### Integrating `related.html` into a layout

Integrating `related.html` into the `default` layout can be as simple as:

{% raw %}
```html
{% comment %}
  <!-- Only show related pages on a page when `related: true` -->
{% endcomment %}
{% if page.id and page.related %}
  <div class="wrapper">
    <h3>See also</h3>
    {% include related.html %}
  </div>
{% endif %}
```
{% endraw %}

### Random Related Pages
The example front matter below will choose 10 random pages across posts and other collections. For example, on a recipe site, you could have a page that just shows random recipies from across your collections and posts.

```yaml
---
title: "Random Recipes"
related_by: random
related_limit: 10
---
```

**Random from the same collection:**

To select random pages from the same collection, you cannot specify `related_by: random` along with `related_from: collection`. Instead relate by 0+ matching tags (i.e. any page from the same collection):
```yaml
related_by: tags         # Match on just tags
related_from: collection # Select from the same collection
related_tags: 0          # Match any page from the collection.
```

**NB: Related posts are chosen randomly at site build time (Jekyll is a static site generator after all). Related pages do not change when the page is accessed, only when the site is built.**
