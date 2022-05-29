git疑问及解决方案
================
目录
----------------
1. [Difference between git remote add and git clone](#question1)
2. [学习git拾遗](#question2)
  1. [如何克隆一个github上自己建立的私有仓库到本地](#question2-1)
  2. [如何强制把远程仓库覆盖到本地仓库](#question2-2)
  3. [github Contributions Calendar不记录](#question2-3)
  4. [初始化本地仓库后添加到远程仓库，远程仓库地址写错了](#question2-4)
  5. [git 添加多个ssh key](#question2-5)
  6. [如何将 blog 同时托管到 gitcafe 和 github 上？](#question2-6)
  7. [为什么配置了 SSH，每次 push 还是需要输入用户名和密码呢？](#question2-7)
  8. [恢复仓库到上一次的提交状态](#question2-8)
  9. [修改最后的提交日志](#question2-9)
3.   

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

 参考：1. [difference-between-git-remote-add-and-git-clone](http://stackoverflow.com/questions/4855561/difference-between-git-remote-add-and-git-clone) <br>

- - -

<h4 id="question2">学习git拾遗</h4>

<h5 id="question2-1">Q1. 如何克隆一个github上自己建立的私有仓库到本地</h5>

命令行输入:
`git clone https://github.com/username/repo`

之会出现: Cloning into "yourrepo"... 之后会提示输入你的Username 和PassWord 输入正确之后就会把私有仓库复制到本地 注意这里输入的是
`https://github.com/username/repo` <br>
不是`git clone git@github.com:username/repo.git`

也不是`git clone git://github.com/myusername/reponame.git`


<h5 id="question2-2">Q2 如何强制把远程仓库覆盖到本地仓库</h5>

`git fetch --all` <br>
`git reset --hard origin/master`

<h5 id="question2-3">Q3 github Contributions Calendar不记录</h5>

查看本地git邮箱：`git config user.email`

然后在github里的accout settings > email里看看你的primary github email 是不是你本地那个邮箱。更改为一样的邮箱：

`git config --global user.email "xx@oo.com"``
<h5 id="question2-4">Q4 初始化本地仓库后添加到远程仓库，远程仓库地址写错了</h5>

添加到远程仓库
`git add remote [name/origin]` [https://github.com/your-name/your-repo]``

修改远程仓库：`git remote set-url --push [name/origin]` [https://github.com/your-name/your-new-repo]

<h5 id="question2-5">Q5 git 添加多个ssh key</h5>

问题描述：我的博客托管在gitcafe 和 github 上，每次 push 都要多次输入用户名密码 灰常麻烦，那么如何添加多个 ssh key 呢。一直没有弄好，在 Cassie 的 github/gitlab 同时管理多个 ssh key启发下，经过 N 多试错，终于大功告成。下面记录下配置配置过程。请注意：我的环境是 windows7 MINGW32 shell 。

1.首先生成一个 SSH keys 供 github 使用：

    $ cd ~/.ssh
    $ ssh-keygen -t rsa -C "your_email@example.com"
执行会让你重命名 key 咱们将其命名为 id_rsa_github 之后会让你输入密码，再次输入密码确认，ok，完成上述步骤就生成了公钥和私钥了。 截图如下：

2.再根据上述步骤生成另一个 SSH keys（根据你自己在两个远程仓库的邮箱命输入邮箱地址），我们将其命名为 id_rsa_gitcafe

3.接着 咱们添加生成的私钥，再这之前执行

    $ ssh-agent -s
    $ ssh-add ~/.ssh/id_rsa_github

这时候会出现错误"Could not open a connection to your authentication agent",参考Could not open a connection to your authentication agent

我们执行:

    $ eval `ssh-agent -s`
继续添加 key:

    $ ssh-add ~/.ssh/id_rsa_github
      ssh-add ~/.ssh/id_rsa_gitcafe


4.Ok,私钥添加上了，咱们把公钥放到 github/gitcafe。打开你的 github 账号，找到设置，找到 SSH keys 点击 ADD SSH key。随便取个 Title，什么你不知道如何添加公钥?!

在你的.ssh 目录下 用 Notepad 或 sublime 打开 id_rsa_github.pub，然后复制粘贴保存。

gitcafe 添加 ssh 公钥同理。

5.胜利尽在眼前，我们 try try 下。

    $ ssh -T git@github.com
然后出现了奇怪的东东，布拉布拉，yes/no，不管它 输入yes。接着会看见您的大名，OK，连接成功，gitcafe 同理请将 $ ssh -T git@gitcafe.com。 来上图，感受下:

或者参考 https://stackoverflow.com/questions/23537881/fingerprint-has-already-been-taken-gitlab 解决 gitlab 上多个key 的问题


<h5 id="question2-6">Q6 如何将 blog 同时托管到 gitcafe 和 github 上？</h5>

这个网上教程很多，待我有时间了，我再手把手教同学们，晒下配置文件，感受下：

    [remote "github"]
        url = https://github.com/yourname/yourname
        fetch = +refs/heads/*:refs/remotes/origin/*
    [branch "master"]
        remote = gitcafe
        merge = refs/heads/gitcafe-pages
    [remote "gitcafe"]
        url = https://gitcafe.com/yourname/yourname
        fetch = +refs/heads/*:refs/remotes/orig/*
    [alias]
        publish=!sh -c \"git push github master && git push gitcafe master:gitcafe-pages\"

<h5 id="question2-7">Q7 为什么配置了 SSH，每次 push 还是需要输入用户名和密码呢？</h5>

参考这里http://segmentfault.com/q/1010000000599327

因为你用的是 https 而不是 ssh。 可以更新一下 origin:

git remote remove origin<br/>
git remote add origin git@github.com:Username/Your_Repo_Name.git
注意：如果你的仓库没有add ssh 只是在.ssh上添加了，在你的仓库里还要添加一次这样才会生效。添加步骤同上。可能要等一分钟左右才会生效。

之后你还需要重新设置track branch，比如：

git pull
git branch --set-upstream-to=origin/master master
对于HTTPS方式，你可以在~/.netrc文件里设定用户名密码，不过这样的风险在于密码是明文存放在这个文件里的，比较容易泄露

machine github.com
login Username
password Password
<h5 id="question2-8">Q8 恢复仓库到上一次的提交状态：</h5>
`$ git reset --hard`

<h5 id="question2-9">Q9 修改最后的提交日志：</h5>
`$ git commit --amend`

  参考：1. [学习git拾遗](http://padding.me/blog/2014/11/04/learn-git-snopt/)
