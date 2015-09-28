git疑问及解决方案
================
目录
----------------
1. [Difference between git remote add and git clone](#question1)


    * * *

    ***

    *****

    - - -

    ---------------------------------------


* * *

<h2 id="span">区段元素</h2>

<h3 id="link">链接</h3>

Markdown 支持两种形式的链接语法： *行内式*和*参考式*两种形式。

不管是哪一种，链接文字都是用 [方括号] 来标记。

要建立一个*行内式*的链接，只要在方块括号后面紧接着圆括号并插入网址链接即可，如果你还想要加上链接的 title 文字，只要在网址后面，用双引号把 title 文字包起来即可，例如：

    This is [an example](http://example.com/ "Title") inline link.

    [This link](http://example.net/) has no title attribute.

会产生：

    <p>This is <a href="http://example.com/" title="Title">
    an example</a> inline link.</p>

    <p><a href="http://example.net/">This link</a> has no
    title attribute.</p>

如果你是要链接到同样主机的资源，你可以使用相对路径：

    See my [About](/about/) page for details.   

*参考式*的链接是在链接文字的括号后面再接上另一个方括号，而在第二个方括号里面要填入用以辨识链接的标记：

    This is [an example][id] reference-style link.

你也可以选择性地在两个方括号中间加上一个空格：

    This is [an example] [id] reference-style link.

接着，在文件的任意处，你可以把这个标记的链接内容定义出来：

    [id]: http://example.com/  "Optional Title Here"

链接内容定义的形式为：

*   方括号（前面可以选择性地加上至多三个空格来缩进），里面输入链接文字
*   接着一个冒号
*   接着一个以上的空格或制表符
*   接着链接的网址
*   选择性地接着 title 内容，可以用单引号、双引号或是括弧包着

下面这三种链接的定义都是相同：

	[foo]: http://example.com/  "Optional Title Here"
	[foo]: http://example.com/  'Optional Title Here'
	[foo]: http://example.com/  (Optional Title Here)

**请注意：**有一个已知的问题是 Markdown.pl 1.0.1 会忽略单引号包起来的链接 title。

链接网址也可以用尖括号包起来：


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
