git疑问及解决方案
================
目录
----------------
1. [Difference between git remote add and git clone](#question1)


***

详述
----------------
<!-- #### Difference between git remote add and git clone #### -->
1. <h4 id="question1">Difference between git remote add and git clone</h4>

> git remote add just creates an entry in your git config that specifies a name for a particular URL. You must have an existing git repo to use this.

> git clone creates a new git repository by copying an existing one located at the URI you specify.

> These are functionally similar :<br />
<code> # git clone REMOTEURL foo</code><br />

>and:

 >`# mkdir foo` <br>
` # cd foo` <br>
 `# git init `<br>
 `# git remote add origin REMOTEURL `<br>
 `# git pull origin master `<br>

 > 区别在于  With using `clone`, the .git/config has a [branch "master"] part.we can add it use `git branch --set-upstream master origin/master

 参考：1. [difference-between-git-remote-add-and-git-clone](http://stackoverflow.com/questions/4855561/difference-between-git-remote-add-and-git-clone)
