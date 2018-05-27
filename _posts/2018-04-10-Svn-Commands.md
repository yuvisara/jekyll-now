---
layout: post
title: SVN Cheat Sheet
---

Subversion(svn) is a version control software which works by maintaining a consistent common repository. Local copy of the repository can be checked out and changes to the local copy are considered temporary. And if commited it is updated in the common respository. Each commit is given a unique and incremental revision id. Similar to branches at top level there is trunks which is organised like a folder structure. One can checkout, and commit only a local folder by giving the appropriate url. Whole repository need not be checkout which is great. 

Note: changes are permanent and need to make another commit to revert the changes.

To checkout a repository:
```shell
svn checkout --username <username> <repo url>
```

https://www.tutorialspoint.com/svn/svn_fix_mistakes.htm

To cleanup the folder:
```shell
svn cleanup
```

To go Back to the original changes:
```shell
svn revert -R .
svn status --no-ignore | grep -E '(^\?)|(^\I)' | sed -e 's/^. *//' | sed -e 's/\(.*\)/"\1"/' | xargs rm -rf
```

To create and apply patches:

```shell
svn diff > PatchFileName.patch
cd DifferntCheckout
svn patch PatchFileName.patch
```

To commit:
```shell
svn commit -m "Commit messages"
```


To see commit log:
```shell
svn log -l 10 
```

To apply svn patch into a git workspace:
```shell
patch -p0 < PatchFileName.patch
```

To get the diff between two revisions:
```shell
svn diff -r 22144:22143
```

To keep the changes in the working copy.
```shell
svn resolve --accept=working fileLocation
```

To add a file/folder to the statging
```shell
svn add FileLocation 
svn rm --keep-local folder_name
```

To update to a specific revision:
```shell
svn update -r 22273
```
