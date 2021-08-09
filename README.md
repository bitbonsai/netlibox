# Netlibox

Netlify + Dropbox  11ty as a blogging platform. Read the [original post from jimniels blog post on Netlify’s blog](https://www.netlify.com/blog/2018/10/15/combining-netlify-with-dropbox-for-a-one-click-publishing-process/).

## How To

- Fork this repo.
- Create [a new site in Netlify](https://app.netlify.com/start) tied to your forked repo.
  - You don’t need to specify any build options through the Netlify UI, we’ll do all of that through through our [`netlify.toml` file](<(https://www.netlify.com/docs/netlify-toml-reference/)>)
- [Setup an app](https://www.dropbox.com/developers/apps) in your Dropbox account.
  - For access type, I like to choose an ”App Folder“ because then API access will be scoped specifically to a single folder (as opposed to my entire Dropbox). This folder will contain the content that gets pulled in at build time.
- Generate an access token for your Dropbox app
  - Find your app in the [Developer App Console](https://www.dropbox.com/developers/apps) then under `Oauth2` click the "Generate" access token button.
- Save your access token as `DBX_ACCESS_TOKEN` in two places:
  - Netlify UI:
    - Your site > Settings > Build & deploy > Build environment variables
  - Local `.env` file:
    - Copy `.env.example` to `.env` and replace the value of `DBX_ACCESS_TOKEN`
- Create a Netlify build hook URL
  - Settings > Build & deploy > Build Hooks > Add build hook
  - For convenience, name it `NETLIFY_BUILD_HOOK_URL`
- Save your Netlify build hook URL as `NETLIFY_BUILD_HOOK_URL` in two places:
  - Netlify UI
    - Your site > Settings > Build & deploy > Build environment variables
  - Local `.env` file
    - Replace the value of `NETLIFY_BUILD_HOOK_URL`
- Open the app console in Dropbox and add the `dropbox-webhook` function URL that Netlify automatically creates for us.
  - This should match a pattern like `https://YOUR_SITE_SUBDOMAIN.netlify.com/.netlify/functions/dropbox-webhook`
  - You can verify this function URL is live by going to "Functions" in your site in Netlify. You should see a function named `dropbox-webhook.js` and if you click on it, you'll be able to see the function URL endpoint.
- Put your `assets` folder in the `.env` file

![final-view](https://user-images.githubusercontent.com/1316441/46992107-c9592f00-d0c5-11e8-8a1c-fa751765a402.png)

Once you've done all this, you should be able to create a markdown file in Dropbox and see it get deployed to your Netlify site's URL.

### Note: binary data
The main difference between this and the original project from jimniels (besides the fact that it uses 11ty instead of Jekyll) is that it not only gets `.md` files, but anything that is on dropbox; I needed to get images as well.

### Note: File Structure

This project is setup to use `11ty`, which means either you use 11ty's naming convention, or you configure your `.eleventy.js` file with your defaults. Check `.env` to see how I've configured that.

```
dropbox-app-folder/
  - whatever.md
  - some-image.jpg
```

### Note: Post Metadata

This site isn't doing anything more than reading the content of each plain-text markdown file and generating the HTML pages. None. Additional/overriding metadata is possible by using [front-matter](https://jekyllrb.com/docs/front-matter/).

Because this example repo is using 11ty, you could write YAML front matter in each markdown file and that would be parsed by 11ty at build time and become available in the site template files.

## Ideas for Future Enhancements

This is a really basic prototype of what you could do with Netlify + Dropbox + 11ty for publishing content to the web. Here are a few additional ideas for enhancing this scaffolding (which, in theory, would work):

- Have a "drafts" deploy preview using Netlify branches