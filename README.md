## GitHub Pages Subdomain Takeover POC

**TL;DR:** Only two things really matter:
1. The target domain has a CNAME DNS record to [anything].github.io
2. GitHub Pages allows you to set a custom domain to [target domain]

## Example

Target: api-docs.rtheory.net

 - nslookup/dig shows this subdomain has a CNAME record to "doesntmatter.github.io"
 - There is a 404 page at doesntmatter.github.io, so there is a *chance* of subdomain takeover.

Takeover Steps:

 1. Login with your own GitHub account (your username can be anything)
 2. Create a repository (the repository name can be anything)
 3. Create a basic HTML file in this repository (content can be any valid HTML)
 4. Go to the settings page for this repository. Scroll down to the GitHub Pages section
	 a. Set the source to your main branch
	 b. Set custom domain to your target (api-docs.rtheory.net in this example)

At this point, one of two things will happen:
- GitHub Pages will give you a success message. Browsing to your target URL will now display the page you created. 
- GitHub Pages will error, saying that the CNAME is already taken. This means that someone (the original owner, or any other individual with good or bad intent) already has this target registered as a custom domain in GitHub Pages, so it will not allow you to register it a second time. Creating your own CNAME file with the target domain in your repository will not bypass this error. 

This repository shows a successful example. 

 - Target: api-docs.rtheory.net
 - Target has a CNAME DNS record: doesntmatter.github.io
 - This repository is named: couldbeanything
 - This repository is set to custom domain: api-docs.rtheory.net, which was available
 - Takeover succeeded

