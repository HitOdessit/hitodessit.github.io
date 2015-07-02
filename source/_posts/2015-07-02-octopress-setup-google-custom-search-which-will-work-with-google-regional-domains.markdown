---
layout: post
title: "[Octopress] How to setup Google Custom Search for Octopress which will work with Google regional domains"
date: 2015-07-02 20:01:40 +0300
comments: true
categories: [octopress, web, search, blog]
---


Octopress has built-in feature - “simple_search”, which by default assigned to *Google Custom Search*. It intended to provide Google search only among pages of your website, no other websites from the web should be included in search results. Feature is configured in `_config.yml` file in your Octopress root folder. By default it has the following value:

```ini
simple_search: https://www.google.com/search
```

Everything looks good, but when I’ve tried to search something using this feature I’ve faced problem - when my browser trying to open `google.com` domain, it automatically redirects this request to my regional domain (in my case `google.com.ua`). So far so good, but during that redirection request loses one of the important parameters - your website among which Google must search! So, in response you get search results among all the web, which, of course, is not what we wanted. 

Changing Google search preferences of my personal account is not solving the problem, because visitors of your site still may face the same problem.
Short “googling” didn’t give any results, so I’ve found my own solution - change `simple_search:` parameter value to different Google subdomain, in which case request preserve all original parameters. Here is my value for this param:
<!--more-->

```ini
simple_search: https://encrypted.google.com/search
```

If you’re using `encrypted.` subdomain, Google doesn’t redirect your request to a regional subdomain, which solves the problem.

Hope this will help someone.
