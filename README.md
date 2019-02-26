# Breadcrumb plugin for Craft CMS 3.1

Build a simple breadcrumb trail based on your current URL

## Requirements

This plugin requires Craft CMS 3.1 or later.

## Installation

To install the plugin, follow these instructions.

1. Open your terminal and go to your Craft project:

        cd /path/to/project

2. Then tell Composer to load the plugin:

        composer require youandmedigital/breadcrumb

3. In the Control Panel, go to Settings → Plugins and click the “Install” button for Breadcrumb.

## Breadcrumb Overview

Breadcrumb allows you to build a breadcrumb trail with little effort. It works by generating a trail based on your current website URL. It is perfect for websites that have pretty and meaningful URL's.

There is no need to worry about setting up a breadcrumb for sections and another for categories or even tags.

For example, if you setup a breadcrumb and your website URL looks like this:
```
https://mysite.local/posts/categories/example-category
```

Breadcrumb would automatically generate the following array for you to template with Twig:
```
array (size=4)
  0 =>
    array (size=3)
      'title' => string 'Home' (length=4)
      'url' => string 'https://mysite.local' (length=18)
      'position' => int 1
  1 =>
    array (size=3)
      'title' => string 'Posts' (length=5)
      'url' => string 'https://mysite.local/posts' (length=24)
      'position' => int 2
  2 =>
    array (size=3)
      'title' => string 'Categories' (length=10)
      'url' => string 'https://mysite.local/posts/categories' (length=35)
      'position' => int 3
  3 =>
    array (size=3)
      'title' => string 'Example Category' (length=11)
      'url' => string 'https://mysite.local/posts/categories/example-category' (length=52)
      'position' => int 4
```

With this array, you could template it using Twig. Here's a basic example:

```
{% set breadcrumb = craft.breadcrumb.config %}

{% if breadcrumb %}
<div class="c-breadcrumb">
    <ol class="c-breadcrumb__items">
        {% for crumb in breadcrumb  %}
            {% if loop.last %}
            <li class="c-breadcrumb__item">
                <span>{{ crumb.title }}</span>
            </li>
            {% else %}
            <li class="c-breadcrumb__item">
                <a class="c-breadcrumb__link" href="{{ crumb.url }}">
                    <span>{{ crumb.title }}</span>
                </a>
            </li>
            {% endif %}
        {% endfor %}
    </ol>
</div>
{% endif %}
```

## Configuring Breadcrumb

You can use Twig to work magic on the Breadcrumb array, but the plugin also comes with these settings:

```
{# If entry variable is empty, try category, tag and finally return null #}
{# This works with customFieldHandleEntryId and customFieldHandle #}
{% set entry = entry ?? category ?? tag ?? null %}

{# Breadcrumb settings array #}
{% set settings =
    {
        homeTitle: 'Home',
        homeUrl: 'https://example.com',
        skipUrlSegment: 1,
        customFieldHandleEntryId: entry.id,
        customFieldHandle: 'myCustomField',
        limit: '3'
    }
%}

{# Settings array passed into the Breadcrumb config #}
{% set breadcrumb = craft.breadcrumb.config(settings) %}
```
- **homeTitle** `(string, optional, default 'Home')`: This allows you to customise the title of the first item in the breadcrumb

- **homeUrl** `(string, optional, default '@baseUrl')`: This allows you to set a custom URL for the first item in the breadcrumb

- **skipUrlSegment** `(int, optional, default 'null')`: This allows you to remove a segment from the Breadcrumb array. For example, if you have the URL `https://mysite.local/posts/categories/example-category` and want to remove `categories` from the breadcrumb, you would enter `3` as a value. This would remove the 3rd segment from all URL's, so be careful!

- **customFieldHandleEntryId** `(int, optional, default '0')`: Works with customFieldHandle. Nothing to customise here.

- **customFieldHandle** `(string, optional, default 'null')`: This allows you to specify a custom field that contains a custom title you want to appear in the breadcrumb. This only works for the last item in the breadcrumb. Requires customFieldHandleEntryId to work.

- **limit** `(int, optional, default 'null')`: If set, will limit the amount of results returned in the Breadcrumb array.

## Is Breadcrumb right for me?

If you have a URL like `https://mysite.local/posts/categories/example-category`, it will generate a breadcrumb based on each segment in the URL. This means if you don't have a template or redirect setup for `https://mysite.local/posts/categories` it will return a 404 when clicked from the breadcrumb trail.

If you have a url that looks like `https://mysite.local/c/12/random/post-title`, Breadcrumb is not for you.

If you need to pull in a custom field to generate each title, Breadcrumb is not for you. Titles are generated from the URL. You can customise the last url segment only.

If you have a multilingual site setup, Breadcrumb will add the language segment to the crumb. This can be fixed by utilising the `homeTitle`, `homeUrl` and `skipUrlSegment` settings.


## Breadcrumb Roadmap

Some things to do, and ideas for potential features:

* Release it

Brought to you by [You & Me Digital](https://youandme.digital)
