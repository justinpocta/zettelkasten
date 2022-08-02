# Netlify Hosting

 [![Netlify Status](https://api.netlify.com/api/v1/badges/47f8bcc0-636e-4f67-be9d-f5337d276215/deploy-status)](https://app.netlify.com/sites/hilarious-sherbet-270247/deploys)

**2022-08-01** - FYI to anyone checking out my Github, I had a lot of trouble as a designer / very non-pro developer. I spent the weekend trying to understand how to get this site to run locally on my M1 mac. I ended up using RVM to manage a pre-Ruby 3.0 version due to what I assume were some gems that weren't able to run on newer versions (listen-3.2.1 stated it was incompatible with ruby 3.0.0p0). I didn't have luck getting Ruby 2.2.7 to work (noted in the Gemlock file), so I landed on a slightly newer version of Ruby 2.6.6 hoping the gems would work with it. Then I got Bundler:2.1.4 installed, had trouble with **eventmachine** and **http_parser.rb** and **ffi** gems but somehow forced them through. I got Jekyll 4.0 installed. Then to make Ruby 2.6.6 load on my M1, I used a method that I don't understand but read here on [Github Issues](https://github.com/rbenv/ruby-build/issues/1699) `RUBY_CFLAGS=-DUSE_FFI_CLOSURE_ALLOC rbenv install 2.6.6`. I finally got Bundle Install to finish and then ran into a TypeError with ffi. Looking around I learned adding `exclude: [vendor]`  to `_config.yml` would help to skip or handle an **Invalid Date** issue when running `jekyll serve` command. Anyway, no idea if any of that makes sense, but this was hard to get running locally for me and for now, it appears to be running!

---

## Update !

Hi everyone ! I've been very busy lately, so I didn't check all of the issues and the PR, but as I have more free time now I'll restart working on the project. Thanks for all the kind messages by the way!

## What is this?

A digital garden using a custom version of `simply-jekyll`, optimised for integration with [Obsidian](https://obsidian.md). It is more oriented on note-taking and aims to help you build a nice knowledge base that can scale with time. 

**Demo is here: [notenote.link](https://notenote.link)**

If you want to see a more refined example, you can check my notes (in french) at [arboretum.link](https://www.arboretum.link/). Build time is approx. 15 seconds, FYI.

Issues are welcome, including feedback ! Don't hesitate to ask if you can't find a solution. 💫

![screenshot](/assets/img/screenshot.png)

## What is different?

- Markdown is fully-compatible with Obsidian (including Latex delimiters!)
- There are now only notes (no blog posts).
- There are cosmetic changes (ADHD-friendly code highlighting, larger font, larger page)
- Code is now correctly indented
- Wikilinks, but also alt-text wikilinks (with transclusion!) are usable.

## How do I use this?

You can click on this link and let the deploy-to-netlify-for-free-script do the rest !

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/Maxence-L/notenote.link)

Follow the [How to setup this site](https://notenote.link/notes/how-to-setup-this-site) guide, written by [raghuveerdotnet](https://github.com/raghuveerdotnet) and then adapted for this fork.

If you want to use it with Github Pages, it is possible, [please read this](https://github.com/Maxence-L/notenote.link/issues/5#issuecomment-762508069).

## How can I participate?

Open an issue to share feedback or propose features. Star the repo if you like it! 🌟

## How do I customize this for my needs?

Things to modify to make it yours:

- Meta content in [\_layouts/post.html](_layouts/post.html):
    ```html
    <meta content="My linked notebook" property="og:site_name"/>
    ```
- The favicon and profile are here: [assets/img/](assets/img/)
- The main stuff is in [\_config.yml](_config.yml):
    ```yaml
    title: notenotelink.netlify.com
    name: notenote.link
    user_description: My linked notebook

    notes_url: "https://notenotelink.netlify.com/"
    profile_pic: /assets/img/profile.png
    favicon: /assets/img/favicon.png
    copyright_name: MIT

    baseurl: "/" # the subpath of your site, e.g. /blog
    url: "https://notenotelink.netlify.com/" # the base hostname & protocol for your site, e.g. http://example.com
    encoding: utf-8
    ```
- You may want to change the copyright in [\_includes/footer.html](_includes/footer.html):
   ```html
   <p id="copyright-notice">Licence MIT</p>
   ```

## How do I remove the "seasons" feature for the notes?

Delete what's inside [\_includes/feed.html](_includes/feed.html) and replace it with:

```liquid
{%- if page.permalink == "/" -%}
    {%- for item in site.notes -%}
        <div class="feed-title-excerpt-block disable-select" data-url="{{site.url}}{{item.url}}">
            <a href="{{ item.url }}" style="text-decoration: none; color: #555555;">
            {%- if item.status == "Ongoing" or item.status == "ongoing" -%}
                <ul style="padding-left: 20px; margin-top: 20px;" class="tags">
                    <li style="padding: 0 5px; border-radius: 10px;" class="tag"><b>Status: </b>{{item.status | capitalize }}</li>
                </ul>
                <p style="margin-top: 0px;" class="feed-title">{{ item.title }}</p>
            {%- else -%}
                <p class="feed-title">{{ item.title }}</p>
            {%- endif -%}
                <p class="feed-excerpt">{{ item.content | strip_html | strip | escape | truncate: 200}}</p>
            </a>
        </div>
    {%- endfor -%}
{%- endif -%}
````

On command-line, you can run `bundle exec jekyll serve` then go to `localhost:4000` to check the result.

## What's coming?

- [Open-transclude](https://subpixel.space/entries/open-transclude/) integration in the template, if possible.
- Different themes! - Please tell me which you'd like to have!
