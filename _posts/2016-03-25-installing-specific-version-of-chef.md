---
layout: post
title: "How to install a specific version of Chef"
excerpt: "In case you want to downgrade the version of chef-client on your machine using the omnibus installer, read below. ."
categories: [Chef]
comments: true

---

In case you want to downgrade the version of chef-client on your machine using the omnibus installer, read below. Occasionally the latest version of Chef Client is incompatible with certain cookbooks or recipes.
On a host where you want to install an older version, use the following command:

```bash
sudo true && curl -L https://www.opscode.com/chef/install.sh | sudo bash -s -- -v 12.9.38
service chef-client restart
```
