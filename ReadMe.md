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