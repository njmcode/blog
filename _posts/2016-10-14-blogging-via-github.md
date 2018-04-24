---
layout: post
title: "Blogging via GitHub"
summary: "A quick guide to minimal blogging with text files and Git."
date: 2016-10-14
image:
    main:
        src: /img/headers/jekyll-github.png
        credit: http://bitbyteyum.com/
---
I was an active, albeit angsty, blogger in my youth. Now that I actually have a profession I'm passionate about, I've been
meaning to pick it up again.  Sadly I've been put off by the various blog platforms available -
they're too complex, bloated and/or costly for my tastes. All I need is somewhere to quickly bang out notes and musings,
nothing fancy.

Since I'm a developer, I'm actually more comfortable in a plain text file than I am in a
CMS or blog engine. And so, after some research, I gravitated towards a
combination of **Jekyll** and **GitHub Pages**.  Here's an overview of how I set it
up - you can also [check out the source][blogsrc].

_(Note - I'm assuming a working knowledge of Git, and that you have a GitHub account and a Git client set up.)_

### Hosting via GitHub ###

[GitHub][gh] has a free feature known as **GitHub Pages** which allows you to serve static
files and assets [from your GitHub repos][ghpages] - very handy for hosting project documentation,
examples, download pages and so on.

I've used this feature in the past to host [my portfolio page][folio], with my `(username).github.io` repo serving
out to `https://(username).github.io`. To update my site, I just commit changes and push to `master`.

I wanted to do the same thing for a simple blog, without affecting my current portfolio
site. GitHub [supports different types][ghpages-help] of GitHub Pages - you can create them them per-user,
per-organization, and per-project.  _Per-project_ was what I was looking for -
I would create a new project repo, tell GitHub to serve from it, and I would have a blog hosted on my _github.io_
domain but separate from my portfolio.

### Repo setup ###

I created a new blank repo for the blog on my GitHub account. Since I wanted
the public URL to be `https://(username).github.io/blog`, I simply named the
repo `blog`.

GitHub can be made to serve out of a repo's `master` branch via the _Settings_ panel
for the repo, under the **Options** > **GitHub Pages** section.

[GitHub lets you configure this][ghpages-config] in a few different ways, including serving
from special branches while keeping `master` for other code - very handy for project docs etc.
I just went for serving out of `master` since the repo is dedicated to the blog.

To test it out, I committed a 'Hello world' `index.html` file and visited `https://(username).github.io/blog` -
sure enough, the file was served sucessfully. Onwards!

### Creating a Jekyll blog ###

**[Jekyll][jekyll]** is a tool that takes templates (for layout) and text files (for content), and compiles them
into static HTML that can be hosted anywhere, without the need for installation, dynamic server-side languages, databases, or any of that guff.
It's perfect for my use case - no-frills, text-based blogging with full control of presentation.

Jekyll is [officially supported][ghpages-jekyll] by GitHub, to the point where Jekyll-format files pushed to a GitHub Pages-enabled repo will be
automatically compiled and served. We just need to install Jekyll locally for development and preview.
(Unfortunately it requires Ruby - we can't have everything, right?)

### The dev environment ###

The [official GitHub instructions for installing Jekyll][ghpages-jekyll-inst] failed me somewhat, but I was eventually
successful. If you have trouble, check the following:

 * Ensure you have [Ruby installed][ruby], along with the dev dependencies for your system.
 * You may also need to install `gcc`, `make`, and other `build-essential` packages, as well as `nodejs` (for some insane reason).

The docs also advise to generate a skeleton Jekyll site via the command line, but I found this more of a hindrance than a help.
Instead, I did the following (taken from
[this great article][jekyll-article] by [Jonathan McGlone][jmcg], and the [official Jekyll docs][jekyll-docs]):

 * Create a `.gitignore` and make sure the `/_site` dir (where Jekyll outputs to) is ignored.

~~~ bash

# ./.gitignore

_site

~~~

 * Create a `_config.yml` in the root with `name` and `description` values listed. We'll reference these in our template files.

~~~ yaml

# ./_config.yml

name: 'My Blog'
description: 'Some random thoughts'

~~~

 * Create a `/_layouts` directory with a `default.html` and `post.html` template, using [Liquid tags][liquid] to insert data such
    as `site.name` (from the `_config.yml` file) and `page.title` (from blog data fields).

~~~ bash

 - /_layouts
    - default.html
    - post.html
 - .gitignore
 - _config.xml
 - Gemfile

~~~

~~~ html

<!-- ./_layouts/default.html -->
{% raw %}
<html>
<body>
    <h1>{{ site.name }}</h1>
    <p>{{ site.description }}</p>
    {{ content }}
</body>
</html>
{% endraw %}
~~~

~~~ html

<!-- ./_layouts/post.html -->
---
layout: default
---
{% raw %}
<!--
This layout extends 'default.html', and populates the
{{ content }} placeholder of that template with its own markup.
-->
<div class="post">
    <h2>{{ page.title }}</h2>
    <p>Posted on {{ page.date | date_to_string }}</p>
    <!-- Blog post body gets injected below -->
    {{ content }}
</div>
{% endraw %}
~~~

 * Create a `/_posts` directory containing [Kramdown][kram] files for your content. The filenames determine URL date
 and slug when published.

~~~ bash

 - /_layouts
    - default.html
    - post.html
 - /_posts
    - 2016-10-14-my-blog-post-slug.md
 - .gitignore
 - _config.xml
 - Gemfile

~~~

~~~ markdown
{% raw %}
<!-- ./_posts/2016-10-14-my-blog-post-slug.md -->
---
layout: post
title: "My first blog post"
date: 2016-10-14
---

This is my blog post content, written in Kramdown. It will get converted to
HTML by the Jekyll build process, and inserted into the {{ content }} tag of
the `post.html` template for rendering.

### A heading ###

Some **bold** and __italicised__ text, and so on.
{% endraw %}
~~~

 * Create an `index.html` in the root to act as a home page and list your posts.

~~~ bash

 - /_layouts
    - default.html
    - post.html
 - /_posts
    - 2016-10-14-my-blog-post-slug.md
 - .gitignore
 - _config.xml
 - Gemfile
 - index.html

~~~

~~~ html

<!-- ./index.html -->
---
layout: default
title: Welcome to my blog!
---
{% raw %}
<!--
This layout extends 'default.html', and populates the
{{ content }} placeholder of that template with its own markup.
-->
<ul>
    {% for post in site.posts %}
    <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
    {% endfor %}
</ul>
{% endraw %}
~~~

 * Run the Jekyll server, which will watch your files, build and serve them from the generated `/_site` dir.

~~~ text

bundle exec jekyll serve

~~~

Then visit `localhost:4000` to view your local site. It won't look like much since we have no CSS styles added, but hey,
it's something!

### Adding some style ###

Getting some style into the blog is as simple as adding a stylesheet to the repo and referencing it in your default
template. I put plain CSS files for normalize, styling and fonts into a `/css` dir in the repo, then added `<link>`
tags in the `<head>` of my `default.html` template to load them in. Way better looking. You _can_ get themes for Jekyll
online, but why go the simple route when you can roll your own abomination?

### Gotcha: making URLs work ###

The above works great on localhost, but if you were to commit and push to `master` now and visit `https://(username).github.io/blog`,
the CSS and post links would likely be broken.

This is because I've chosen to use a GitHub Project Page.  The URL of the hosted Page is `/blog`, but Jekyll resolves all
links to the root domain. We can set a `baseurl` in our `_config.yml` which would fix this for us on GitHub, but would break
things on local. Fortunately, we can work around this.

 * Edit your `_config.yml` and add a `baseurl` corresponding to our blog repo name:

~~~ yaml

# ./_config.yml

name: 'My Blog'
description: 'Some random thoughts'
baseurl: /blog

~~~

 * Update your CSS and anchor tag links to use the `baseurl` property, like so:

~~~ html
{% raw %}
<!-- For asset tags e.g. <link>, <script>, etc. Note the slash after site.baseurl ! -->
<link rel="stylesheet" href="{{ site.baseurl }}/css/main.css">

<!-- For blog links etc. -->
<a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
{% endraw %}
~~~

 * Stop and re-start your local server, but override the `baseurl` so that it uses the root instead:

~~~ text

bundle exec jekyll serve --baseurl ''

~~~

This now means that everything works fine on `localhost:4000` **and** when you upload to GitHub.
Sorted!

### Publishing ###

To go live, simply `git commit` and `git push origin master`, then visit `https://(username).github.io/blog` to view
the live version of your blog in all its glory!

### Improvements and hacks ###

We've only scratched the very surface of what Jekyll can do with data and Liquid templating. A couple of ideas I've had
(and may or may not add in!):

 * **Post summaries** - a one-line overview of the contents of a blog post. You can define any metadata you want at the
 top of each `/_posts` file, so this is as simple as defining a `summary` then referencing it in your layout templates:

 * **Categories / tags** - I haven't explored Jekyll's support for this yet, but I imagine you could do it with some
 custom metadata and clever use of Liquid filters and tags.

 * **'About the author' box** - I created this as a HTML snippet and saved it in `./_includes/author.html`, which allows me
 to call `{% raw %}{% include author.html %}{% endraw %}` inside any template to insert it. Very handy for re-usable HTML components!

 * **Pagination, filtering, sorting etc** - Jekyll does include support for these, so I'll be adding them in future.

Hopefully this has given you some insight into how to get blogging with common dev tools - no monolithic CMS forms,
hosting costs, or wrangling with Wordpress themes!  I'll continue to refine and experiment with the platform for the
next while and see how far I can take it, from a developer and editor POV.  Watch this space.

**- NJM**


[blogsrc]: https://github.com/njmcode/blog
[folio]: https://njmcode.github.io/
[gh]: https://github.io/
[ghpages]: https://pages.github.com/
[ghpages-help]: https://help.github.com/articles/user-organization-and-project-pages/
[ghpages-config]: https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/
[jekyll]: https://jekyllrb.com/
[ghpages-jekyll]: https://help.github.com/articles/about-github-pages-and-jekyll/
[ghpages-jekyll-inst]: https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/
[ruby]: https://www.ruby-lang.org/en/documentation/installation/
[jmcg]: http://jmcglone.com/
[jekyll-article]: http://jmcglone.com/guides/github-pages/
[jekyll-docs]: https://jekyllrb.com/docs/home/
[liquid]: https://shopify.github.io/liquid/tags/
[kram]: http://kramdown.gettalong.org/quickref.html
