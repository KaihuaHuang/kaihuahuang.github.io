---
layout:     post
title:      Git Cookbook
subtitle:   Distributed Version Control
date:       2019-1-4
author:     William
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - git
    - github
---
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: { 
      inlineMath: [['$','$'], ['\\(','\\)']],
      processEscapes: true
    }
  });
  </script>
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

# Basic Operation in Local

![](http://ww1.sinaimg.cn/large/83d6b255gy1fyuhn4e329j20cq06iaa8.jpg)

* **Initialize a Git Repository**
```
git init
```
*This command will create a empty repository. There will be a .git floder which is used for tracking and managing repository.*

* **Adding File from Working Directory to Stage**
```
git add [filename]
```

* **Commit the changes in Stage to Local Repository**
```
git commit -m "[commitment descriptions]"
```
* **Check Current Status**
```
git status
```
* **Show the Differences**
```
git diff
```
* **Discard the Modifications in Working Directory**
```
git checkout --[filename]
```
*Actually, this command replace the file in working directory with the file in repository.*

* **Unstages the file**
```
git reset Head [filename]
```
*Just like delete the file in the stage, but in working directory, the file still preserves its contents.*

* **Remove File**
```
git rm [filename]
```

* **Renamce File**
```
git mv [original filename] [new filename]
```


# Version Control
![](http://ww1.sinaimg.cn/large/83d6b255gy1fyullxubj1j206j04iq2t.jpg)

![](http://ww1.sinaimg.cn/large/83d6b255gy1fyulm52fysj206j04iwed.jpg)

* **Lists Version History for the Current Branch**
```
git log
```

* **Back to Specific Commit**
```
git reset --hard [commit id]
```

# Branch Operation

* **Lists all local branches in the current repository**
```
git branch
```

* **Create new branch**
```
git branch [branch name]
```

* **Branch Switch**
```
git checkout [branch name]
```

* **Merge Another Banch to Current Branch**
```
git merge [branch name]
```

* **Delete Branch**
```
git branch -d [branch name]
```

# Working with Remote Repository
![](http://ww1.sinaimg.cn/large/83d6b255gy1fyulxdbewvj20vt0crtah.jpg)

* **Link Local Repository with Remote Repository**
```
git remote add origin [url]
```

* **Build a Local Repository Copy using Remote Repository**
```
git clone [url]
```

* **Get Remote Repository Infomation**
```
git remote 
```

* **Upload Local Repository Modifications**
```
git push [remote repository name] [remote brunch name]
```
*Default remote repository name is origin*

* **Download Remote Repository Modifications**
```
git pull
```

# Reference
[廖雪峰Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
[Git使用教程](https://blog.csdn.net/qq_36150631/article/details/81038485)