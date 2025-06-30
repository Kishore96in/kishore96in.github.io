+++
title = "Cloning GitHub private repositories using Deploy Keys"
date = "2025-06-30T12:43:55+05:30"
url = "blog/github-private-repos-deploy-keys"
tags = [
	"tech-notes",
	]
+++

Sometimes you want to clone a private repository to a computer without giving it complete access to your GitHub account.
While deploy keys allow this, GitHub only allows a particular deploy key to be used in a single repository.
This post describes a workaround.

<!--more-->

First create a deploy key:
```bash
ssh-keygen -t ed25519 -C "deploy key for blahblah"
```
and save the key as, say, `~/.ssh/key_for_github_blahblah`
Add this as a deploy key in the GitHub settings for your repository.[^github_deploy_key_docs]
If you want, you can give this key push access as well.

[^github_deploy_key_docs]: <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys>

In `~/.ssh/config`, add an entry like
```
Host some_unique_name.github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/key_for_github_blahblah
```

Now, clone your repository using
```bash
git clone git@some_unique_name.github.com:GITHUB_USERNAME/REPOSITORY_NAME.git REPOSITORY_NAME
```
If your deploy key had write access, changes you make in your local clone can also be pushed to GitHub.
