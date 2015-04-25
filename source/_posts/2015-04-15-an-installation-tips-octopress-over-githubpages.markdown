---
layout: post
title: "Installation Tips - Octopress over GitHubPages."
date: 2015-04-15 13:43:48 +1000
comments: true
categories: [Octopress , GitHupPages , Deploy error]
---

I've recently setup my weblog  using Octopress weblog framework over githubpages. I noticed some delicate hints should be applied when setting up Octopress over GitHubPages. When you are trying to install the Octopress weblog framework over your GitHubPages, according to the Octopress installation guide you should fist login into your GitHub account and create a repository and  name the repository with the format username.github.io, where username is your GitHub user name or organization name. now some tips to prevent getting errors like this:

```  
c## Pushing generated _deploy website
To git@github.com:farshi/farshi.github.io.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'git@github.com:farshi/farshi.github.io.git'
To prevent you from losing history, non-fast-forward updates were rejected
Merge the remote changes (e.g. 'git pull') before pushing again.  See the
'Note about fast-forwards' section of 'git push --help' for details.
```

Tip 1: In the page of creating the repository  you should not select 'Initialize this repository with a README'. Unless when you hit rake deploy command you will get this error.

Tip 2: After creating repository you should specify your repository to octopress by hitting this command 'rake setup... ' , when you asked to enter the URI for your repository , stick to the https URL instead of git@ URI. 

I hope it helps you to have a smooth Octopress setup. 

