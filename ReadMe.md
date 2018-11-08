# www.Jering.tech
This project will contain:
- A home page
- Articles
- Indexes for utilities and products
- General pages like the 404 page

## Utility/Product Websites
Each utility/product website should be published to \<project name>.netlify.com. A corresponding redirect from www.jering.tech/\<utilities|products>/\<project name> to each \<project name>.netlify.com site must be added to this project.  

## Sitemaps
This project will contain a robots.txt (hosted at www.jering.tech/robots.txt) with multiple sitemap references (this is legal, according to https://www.sitemaps.org/protocol.html#submit_robots). One reference will point to the sitemap that maps out
this project's pages. The other references will point to sitemaps generated for utility/product websites (hosted at \<project name>.netlify.com/sitemap.xml, ie www.jering.tech/\<utilities|products>/\<project name>/sitemap.xml). Hosting multiple sitemaps at different
paths is legal, according to https://www.sitemaps.org/protocol.html#location.

## Logos
Every utility/product can have its own logo. Use SVGs with height 312px.

## Favicons
The favicon for each utility/product should be the same image as the logo. Use https://realfavicongenerator.net/ to generate favicons and associated files.

## Caching
For maximum efficiency, all sites must use "infinite" browser caching for resources other than `.html` files: `Cache-Control: "public, max-age=31536000"`. When a resource changes, its name must change so that
it is retrieved by the browser. Webpack takes care of bundles in this aspect, appending hashes to their names. We should automate generation of unique names for other resources too, like images and fonts.