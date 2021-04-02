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
 - My GitHub username: rtheory (it doesn't need to match domain name though)
 - This repository is named: couldbeanything
 - This repository is set to custom domain: api-docs.rtheory.net, which was available
 - Takeover succeeded. Notice how the legitimate CNAME record (doesntmatter.github.io), repository name (couldbeanything) and my GitHub username (rtheory) don't have any impact on whether or not this succeeds. The only real requirement is GitHub pages allowing you to set a custom domain on your repository to point to your target domain (api-docs.rtheory.net). 

## FAQ

*Doesn't my username or repository name need to match the target's github.io CNAME record in DNS?*

No. Subdomains of github.io all point to the same IPs/load balancer. For example, abc.github.io and xyz.github.io and doesntmatter.github.io all resolve to the same server. The only important thing is that GitHub Pages allows you to set a custom domain (CNAME file in your repository) that matches your target URL. 

Since all .github.io subdomains resolve to the same server, that server will look across the listing of custom domains it has assigned to see which repository to redirect the visitor to. It doesn't try to match usernames, it doesn't try to match repository names, and it doesn't try to match what the target's DNS CNAME record actually pointed to.


*GitHub Pages says the CNAME is already taken*

After testing, the GitHub Pages CNAME (custom domain assignment) becomes available again:
- Immediately after deletion of the repository
- Immediately after the repository goes private
	- GitHub Pro allows you to have GitHub Pages in a private repository, so if a search for [targetdomain] does not yield any CNAME file results, it must be stored in someone's private repository. You will not be able to set this custom domain until this CNAME file or repository is deleted.
