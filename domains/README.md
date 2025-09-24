# Register an is-a.dev subdomain

A comprehensive guide on how to register your own `.is-a.dev` subdomain.

## Overview

**is-a.dev** is a service that allows developers to get a sweet-looking `.is-a.dev` subdomain for their personal websites. This guide will walk you through the process of creating your own subdomain registration.

## Prerequisites

- A GitHub account
- Basic knowledge of JSON format
- A website or service you want to point your subdomain to

## Step 1: Fork the Repository

1. Navigate to the [is-a-dev/register repository](https://github.com/is-a-dev/register) on GitHub
2. If you don't have a GitHub account, [create one](https://github.com/signup) first
3. Click the **Fork** button to create your own copy of the repository

## Step 2: Creating the JSON File

### Navigate to the domains folder

1. In your forked repository, navigate to the `domains/` folder
2. Click the **Add file** button and select **Create new file**

### Name your file

Name your file using the subdomain you want, followed by `.json`. For example:
- If you want `william.is-a.dev`, name your file `william.json`
- If you want `myproject.is-a.dev`, name your file `myproject.json`

#### Naming Requirements

Your subdomain name must meet the following requirements:
- **Length**: More than 2 characters but less than 64 characters
- **Characters**: Only use standard English letters (a-z), numbers (0-9), and hyphens (-)
- **No underscores**: Only dashes are permitted for special characters
- **No consecutive hyphens**: Avoid using `--` in your subdomain name
- **Lowercase only**: All characters must be lowercase
- **No reserved words**: Cannot use reserved or internal domain names

### JSON File Structure

Your JSON file should follow this structure:

```json
{
    "owner": {
        "username": "your-github-username"
    },
    "records": {
        "RECORDTYPE": "RECORDCONTENT"
    }
}
```

#### Required Fields

- **owner.username**: Your GitHub username (required)
- **records**: DNS records object with at least one record (required)

#### Optional Fields

- **owner.email**: Your email address (optional, but not recommended to use GitHub noreply emails)
- **proxied**: Boolean to enable Cloudflare proxy (optional)
- **redirect_config**: Configuration for URL redirects (optional)

## Step 3: Configure DNS Records

Replace `RECORDTYPE` and `RECORDCONTENT` with the appropriate values for your use case:

### Common Record Types

#### CNAME Record (Most Common)
For GitHub Pages, Netlify, Vercel, and other hosting services:

```json
{
    "owner": {
        "username": "your-github-username"
    },
    "records": {
        "CNAME": "your-github-username.github.io"
    }
}
```

#### A Record
For pointing to an IPv4 address:

```json
{
    "owner": {
        "username": "your-github-username"
    },
    "records": {
        "A": ["192.168.1.1"]
    }
}
```

#### AAAA Record
For pointing to an IPv6 address:

```json
{
    "owner": {
        "username": "your-github-username"
    },
    "records": {
        "AAAA": ["2001:db8::1"]
    }
}
```

#### URL Record
For simple redirects:

```json
{
    "owner": {
        "username": "your-github-username"
    },
    "records": {
        "URL": "https://your-website.com"
    }
}
```

#### TXT Record
For verification or other text data:

```json
{
    "owner": {
        "username": "your-github-username"
    },
    "records": {
        "TXT": ["verification-string", "another-txt-record"]
    }
}
```

### Multiple Records

You can include multiple record types (except CNAME with others unless proxied):

```json
{
    "owner": {
        "username": "your-github-username"
    },
    "records": {
        "A": ["192.168.1.1"],
        "TXT": ["v=spf1 include:_spf.google.com ~all"]
    }
}
```

## Step 4: Advanced Configuration

### Cloudflare Proxy

To enable Cloudflare's proxy features (DDoS protection, caching, etc.):

```json
{
    "owner": {
        "username": "your-github-username"
    },
    "records": {
        "CNAME": "your-website.com"
    },
    "proxied": true
}
```

### URL Redirects

For custom redirect configurations:

```json
{
    "owner": {
        "username": "your-github-username"
    },
    "records": {
        "URL": "https://your-website.com"
    },
    "redirect_config": {
        "redirect_paths": true,
        "custom_paths": {
            "/blog": "/posts",
            "/contact": "/about"
        }
    }
}
```

## Step 5: Submit Your Pull Request

1. After creating your JSON file, scroll down to the bottom of the page
2. Add a commit message like "Add [yoursubdomain].is-a.dev"
3. Click **Commit new file**
4. Click **Create pull request**
5. Fill out the pull request template with the required information
6. Check all the boxes in the requirements checklist
7. Click **Create pull request** to submit

## Step 6: Wait for Review

- Your pull request will be reviewed by the maintainers
- Keep an eye on your PR in case changes are needed
- Once approved and merged, your DNS records will be active within a few minutes
- You can then access your website at `yoursubdomain.is-a.dev`

## Supported Record Types

The following DNS record types are supported:
- **A**: IPv4 address
- **AAAA**: IPv6 address
- **CAA**: Certificate Authority Authorization
- **CNAME**: Canonical name (alias)
- **DS**: Delegation Signer
- **MX**: Mail exchange
- **NS**: Name server (restricted - see below)
- **SRV**: Service locator
- **TLSA**: Transport Layer Security Authentication
- **TXT**: Text record
- **URL**: HTTP redirect

## NS Records - Special Requirements

NS records are restricted and require special justification:

- **Why restricted**: To prevent potential abuse and maintain service quality
- **When needed**: Only when you need to delegate DNS control to your own nameservers
- **How to apply**: Include detailed reasoning in your pull request explaining why you need NS records
- **Alternative**: Consider if other record types can meet your needs first

**Note**: Pull requests adding NS records without sufficient reasoning will be closed.

## Common Examples

### GitHub Pages
```json
{
    "owner": {
        "username": "johndoe"
    },
    "records": {
        "CNAME": "johndoe.github.io"
    }
}
```

### Netlify
```json
{
    "owner": {
        "username": "johndoe"
    },
    "records": {
        "CNAME": "amazing-site-123456.netlify.app"
    }
}
```

### Vercel
```json
{
    "owner": {
        "username": "johndoe"
    },
    "records": {
        "CNAME": "my-project.vercel.app"
    }
}
```

### Simple Redirect
```json
{
    "owner": {
        "username": "johndoe"
    },
    "records": {
        "URL": "https://johndoe.dev"
    }
}
```

## Troubleshooting

### Common Issues

1. **File name doesn't match subdomain**: Ensure your file is named `subdomain.json`
2. **Invalid JSON**: Use a JSON validator to check your syntax
3. **Reserved subdomain**: Some subdomains are reserved for internal use
4. **CNAME conflicts**: CNAME records cannot be combined with other records unless proxied
5. **Invalid characters**: Only use letters, numbers, and hyphens in subdomain names

### Getting Help

- Check the [FAQ](https://docs.is-a.dev/faq/)
- Join the [Discord server](https://discord.gg/is-a-dev-830872854677422150) for community support
- Review existing domain files in this folder for examples
- Read the full [documentation](https://docs.is-a.dev)

## Important Notes

- **Free service**: This is a free service provided to the developer community
- **No SLA**: While we aim for high availability, there's no service level agreement
- **Terms of Service**: Make sure to review the [Terms of Service](../TERMS_OF_SERVICE.md)
- **Abuse reporting**: Report any abuse via [GitHub issues](https://github.com/is-a-dev/register/issues/new?template=report-abuse.md)

## Supporting the Project

If you find this service helpful, please consider:
- ⭐ Starring the repository
- 🐛 Reporting bugs or issues
- 🤝 Contributing to the codebase
- 💬 Helping others in the community

---

**Happy coding with your new .is-a.dev subdomain!** 🎉