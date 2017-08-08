---
layout: post
title: "Removing multiple files from a Git repo that have already been deleted from disk"
date: 2016-12-15
---

```sh
#    deleted:    file1.txt
#    deleted:    file2.txt
#    deleted:    file3.txt
#    deleted:    file4.txt
```

```sh
git ls-files --deleted -z | xargs -0 git rm 
```
