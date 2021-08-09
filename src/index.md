---
title: "DBX"
layout: "base.njk"
---

![{{ meta.title }}](/img/logos.svg)

The content below is being fetched from Dropbox:

<div class="dbx-contents">
<ul>
{% for d in collections.dbx %}
<li><a href="{{d.url}}">{{ d.data.name }}</a></li>
{% endfor %}
</ul>
</div>

# TL;DR
1. Place content (`.md` files, images) on a Dropbox folder
2. This triggers a Netlify build for a 11ty SSG site
3. Go to your site and enjoy!

---

# Context
Building sites is fun. Using Netlify and 11ty, you can quickly build a site and serve on Netlify for free. Just hook yor github repo on Netlify and Github becomes your CMS!

But then, what if you want other people -- not just geeks like you -- to add content? What if you want to quickly write down few words and publish from your phone?

What could be a simpler workflow that is simple and easy enough to share with your mom or a less techy-savvy friend?

## It's not only text, not only pages
Text only sites are quite easy to integrate with Notion, or even Google Sheets.

The problem starts when you try to integrate images, other file types, or a catalog of items to generate cards. The solutions I've found are cumbersome or just too much hassle for the user. Should you put your images on an S3? Google Photos? Copy each link and paste into a spreadsheet or a Notion database?

The ideal solution would be to have a simple folder where you'd dump your content and magically that would appear on the web. But how?

## The use case: a small website for an artist
My wife is an artist. So naturally I wanted to give her a [website to showcase her paintings](https://karinwolff.art) . It doesn't need to be fancy, it's basically a small set of pages, showing her paintings, zooming on them, a simple about page.

That's the best scenario for a SSG, to be hosted on [Netlify](https://netlify.com).

And I want to give her the possibility to update it, with as little hassle as possible.

So I built a 11ty site and started to look how to have a simpler, easier CMS to give her. Pure responsive html, nothing fancy but with 100% Pagespeed score.

Then I found [Netlibox](https://github.com/jimniels/netlibox), that ticked **almost** all boxes. This is the base of this project, and a genius piece of code from [Jim Nielsen](https://github.com/jimniels).

Only thing is I already have the 11ty site, and Netlibox runs on Jekyll, and only supports `.md` files... then Netlibox11ty was born.

---

## How does it work

There are a few requirements:

1. Dropbox account
2. Netlify account
3. Github account

The content editors will need to have a Dropbox account, where you'll create your Dropbox App.

### Initial setup
Follow the setup part of [Netlibox's  How To](https://github.com/jimniels/netlibox).

### How Netlibox11ty is different?

Two major differences:

1. It uses 11ty instead of Jekyll
2. It copies all files to the `dbx` directory, not only text ones

### How does it work?

Any `.md` file will be converted to html. Any other file will be copied to the `_site/dbx` folder, untouched (useful for images or downloadable files).

In this example, a `dbx` `collection` was created by adding the tag `dbx` to frontmatter in all markdown files. Feel free to customize your collections however you want!

Here's an example Frontmatter for my wife's paintings:

<div class="dbx-contents pre">---
name: "Wild Waves"
image: "wild-waves.jpg"
year: 2021
type: "Acrylic on Canvas"
size: "150x101cm"
price: "AUD $"
tags: "dbx"
layout: "page.njk"
---</div>

The main purpose here is to have a `collection` to be looped into cards; The pages for paintings are not linked anywhere, but could be. I've applied a layout just to check if the image is accessible.

And here's the output, looping on the collection:

![iPhone output](/img/iphone-karinart.jpg)

### Why Dropbox?
**Simplicity**. We don't mind commiting stuff to github to update a website, but we're not most users. 

Learning how to write a basic front-matter markdown file can be intimidating enough. With this system, any file stored in Dropbox will trigger a Netlify build, that will fetch the files, place them into a `dbx` folder and copy them to the public site.

The `.md` files are going to be converted to `html`, and the other files will be copied as-is and can be referenced in the collection you've specified in Front-matter.

### Show me!

The example below is in the repo.

You'll find 2 `.jpg` files with my wife's paintings, with the correspondent `.md` files that contains the front-matter that generates the `collection`.

![Dropbox folder](/img/dbx-folder.png)

Any files dropped there will trigger the Netlify build, that will generate the site and publish it.

That's it, really. Drop files, publish site. Can be images, pages as `.md` files, and those can be converted to pages or just generate collections.