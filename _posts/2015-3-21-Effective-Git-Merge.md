---
layout: post
title: How to do git merge with beauty history log
---

For example, We have a branch named `waiportal` dedicated to develop new site that have full accessibility. After finish it; we want to merge this branch into `master` branch (trunk); then we do the steps as below:

Current history of `master` as below:
```
052e7e9 ECMS-3515 
1b491f9 remove sensitive data 
827b77c add sensitive data 
1870a21 remove the svn plugin pom 
```

Current history of `waiportal` branch
```
53e11b7 ECMS-3574 
26583f2 ECMS-3446 
57d0a2b ECMS-2983 
35e6db5 ECMS-3519 
d4f9793 ECMS-3513 
1b491f9 remove sensitive data 
827b77c add sensitive data 
1870a21 remove the svn plugin pom 
```

Then run command to `merge` the `waiporal` onto `master` (`master` is current branch)
```
$ git merge waiportal
```

The history log after merging will be as below
```
b39a42d Merge branch 'waiportal' 
052e7e9 ECMS-3515 
53e11b7 ECMS-3574 
26583f2 ECMS-3446 
57d0a2b ECMS-2983 
35e6db5 ECMS-3519 
d4f9793 ECMS-3513 
1b491f9 remove sensitive data 
827b77c add sensitive data 
1870a21 remove the svn plugin pom 
```

You can se more details with `$git log –oneline --graph`
```
*   b39a42d Merge branch 'waiportal' 
|\  
| * 53e11b7 ECMS-3574 
| * 26583f2 ECMS-3446 
| * 57d0a2b ECMS-2983 
| * 35e6db5 ECMS-3519 
| * d4f9793 ECMS-3513 
* | 052e7e9 ECMS-3515 
|/  
* 1b491f9 remove sensitive data 
* 827b77c add sensitive data 
* 1870a21 remove the svn plugin pom 
```

As merge above, we preserved all the history on `waiportal`, but sometime we need to merge a branch with only one composing commit history for much scattered commits. 
With below data, we have some more commits on `master` and `waiportal` then we want to merge `waiportal` into `master` again.

Current history on `master` branch
```
* 2169678 ECMS-3470 
*   b39a42d Merge branch 'waiportal' 
|\  
| * 53e11b7 ECMS-3574 
| * 26583f2 ECMS-3446 
| * 57d0a2b ECMS-2983 
| * 35e6db5 ECMS-3519 
| * d4f9793 ECMS-3513 
* | 052e7e9 ECMS-3515 
|/  
* 1b491f9 remove sensitive data 
* 827b77c add sensitive data 
* 1870a21 remove the svn plugin pom 
```

Current history on `waiportal` branch
```
* 37cef38 fix the uncaught exception in javascript 
* 067bea0 fix the heading order in content view 
* b32c6b4 modify pom file to upgrade icepdf to 4.3.3 version 
* 53e11b7 ECMS-3574 
* 26583f2 ECMS-3446 
* 57d0a2b ECMS-2983 
* 35e6db5 ECMS-3519 
* d4f9793 ECMS-3513 
* 1b491f9 remove sensitive data 
* 827b77c add sensitive data 
```

We have two ways to do this by using `git merge –squash` or by using `rebase -i` to merge `waiportal` branch into `master` branch with one commit for all comits of  b32c6b4,  067bea0 and  b32c6b4

## Using `git-merge`

  > Make sure that you are on `master` branch, if not run `$ git checkout master` before running two below commands

```
$ git merge –squash waiportal
$ git commit -m "super one commit of waiportal"
```

Then three commits  `b32c6b4`,  `067bea0` and  `b32c6b4` of waiportal branch will be merged into `master` branch in one commit `72847b0`, you can see more details by digging the history log on master branch, you may see the history of master like below:

```
* 72847b0 super one commit of waiportal
* 2169678 ECMS-3470
*   b39a42d Merge branch 'waiportal'
|\  
| * 53e11b7 ECMS-3574
| * 26583f2 ECMS-3446
| * 57d0a2b ECMS-2983
| * 35e6db5 ECMS-3519
| * d4f9793 ECMS-3513
* | 052e7e9 ECMS-3515
|/  
* 1b491f9 remove sensitive data
* 827b77c add sensitive data
```

## Using interative `rebase`

  1. switch to waiportal branch
    ```
    $ git checkout waiportal
    ```
  
  2. rebase with interactive mode the waiportal branch on master branch
    ```
    $ git rebase -i master
    ```

  > Then git will list all the commits on waiporal that un-merged with master branch, we can use squash of rebase to modify the history and much more things (ref rebase part)

  ```
  pick b32c6b4 modify pom file to upgrade icepdf to 4.3.3 version
  pick 067bea0 fix the heading order in content view
  pick 37cef38 fix the uncaught exception in javascript
  ```
  
  We want to merge three above commits into only one commit, so we modify the history as below
  ```
  ```










