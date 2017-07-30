---
layout: post
title: "How to read large directories with many files"
excerpt: "Occasionally I come across directories with so many files that ls just hangs."
categories: [Administration]
comments: true

---

Occasionally I come across directories with so many files that ls just hangs. This is because the listing process is holding the file names in memory so that it can sort them. You can use the ls -1 -f options to show the files immediately. -1 will list on e file per line and -f will stop sorting.

If you want to remove all the files in a directory you could try the following command:

```bash
ls -1 -f | xargs rm
```

After cleaning up very many unwanted files, you will be left with a huge and sparse directory object. Three million files in one directory, for example, apart from taking up space in themselves, will likely push the directory object to occupy over 100 Mb of space.

You might decide to recreate the directory to reclaim that 100 Mb. But if it is /tmp, then do so with great care, and only in single user mode.
