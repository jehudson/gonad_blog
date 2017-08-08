---
layout: post
title: "How to delete files older than x days"
excerpt: "The find utility on Linux allows you to pass a range of arguments including one to execute another command on each file."
categories: [Administration]
comments: true

---

The find utility on Linux allows you to pass a range of arguments including one to execute another command on each file. We can use this feature in order to figure out what files are older than a certain number of days and the use the rm command to delete them.

## Command Syntax

```bash
find /path/to/files* -mtime +5 -exec rm {} \;
```

Note that there are spaces between rm, {} and \;

## Explanation

  * The first argument is the path to the files. This can be a path, a directory or a wildcard as in the example above. I would recommend using the full path and and make sure that you run the command without the exec rm first to make sure that you are getting the right results.
  * The second argument, -mtime, is used to specify the number of days old the the file is. If you enter +5, it will find files older than five days.
  * The third argument, -exec, allows you to pass in a command such as rm. The {} \; at the end is required to end the command.
