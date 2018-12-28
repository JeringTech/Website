# Jering.Website
This is the repository for www.jering.tech. The site consists of:
- A home page.
- Indexes for content produced by Jering. Content is divided into the following categories:
  - Articles
  - Utilities
    - Tools released under permissive, open source licenses.
  - Guides
    - A guide is a collection of articles organized as a coherent exposition on a topic.
  - Products
- 404, contact, privacy policy and licenses pages.

## Structure

### Utility/Guide/Product Websites
Each utility/product website must be published to \<project name>.netlify.com. A corresponding redirect from www.jering.tech/\<utilities|guides|products>/\<project name> to each \<project name>.netlify.com site must be added to this project's netlify.toml.

### Sitemaps
This project contains a robots.txt (hosted at www.jering.tech/robots.txt) with multiple sitemap references. This method is described [here](https://www.sitemaps.org/protocol.html#submit_robots).  

One reference points to the sitemap that maps out this project's pages. The other references point to sitemaps generated for utility/guide/product websites (\<project name>.netlify.com/sitemap.xml). This method is 
described [here](https://www.sitemaps.org/protocol.html#location).

## Logos
Every utilities and guides use the Jering logo. Products must have their own logos.

## Favicons
The favicon for each website must be the same as its logo. Use https://realfavicongenerator.net/ to generate favicons and associated files.

## Caching
For maximum efficiency, all sites use "infinite" browser caching for resources other than `.html` files: `Cache-Control: "public, max-age=31536000"`. When resources change, their name must change so that
it is retrieved by the browser (cache busting). Webpack takes care of bundles in this aspect, appending hashes to their names. We should automate generation of unique names for other resources too, like images and fonts.