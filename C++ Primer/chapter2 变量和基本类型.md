Charpter 2 变量和基本类型
======================================
1. 整型int.short,long 都默认是带符号型，即signed,如果需要无符号，则
  声明为unsigned

2. 当试图赋值超限时，对于unsigned类型，编译器对unsigned类型的可能取值数目求模，然后取所得值。

   unsigned char可能取值0~255，数目为256，所以336存储时，336%256=80，所以存储为80

   signed char，按照具体编译器实现，不过一般和unsigned一样，这里要注意负值，也是取模
   如(-1)%256=255

   这里注意负值的取模方法：<a href="#ct1">见补充</a>

3. int数值后加L，表示long(不要加l,容易和1混淆)，加U表示unsigned,可以同时加LU

4. '\0'是“空字符”，是一种'\000',八进制数字转义的一种字符，一类的还有'\12',换行符.....,同样也可以用'\xddd'，十六进制表示

5. 为了兼容C语言，C++所有的字符串字面值都由编译器自动在末尾加一个空字符

6. 对象是什么？

   对象是内存中具有类型的区域，可以使用对象描述程序中的数据，而不管这些数据是内置类型还是类类型。

7. C++变量名大小写敏感

8. 同一类型的在同一行一起初始化是一个好习惯，C++中“赋值不是初始化”，初始化指创建变量并且赋初始值，赋值是指擦出对象当前值，并用新值代替

9. std表示standard，标准库

10. 相比于复制初始化，直接初始化更灵活效率也更高

11. 


补充
==============================================
* <div id="ct1">
<p><h3>负值的取模方法：</h3></p>
<p>最近在一道 Java 习题中，看到这样的一道题：</p>
<blockquote>
<p>What is the output when this statement executed: &nbsp;System.out.printf(-7 % 3);</p>
</blockquote>
<p>正整数的取余运算大家都很熟悉，但是对于负数、实数的取余运算，确实给人很新鲜的感觉。于是我对此进行了一些探索。我发现，这里面还是颇有一点可以探索的东西的。</p>
<p><strong>自然数的取模运算</strong>的定义是这样的（<strong>定义1</strong>）：</p>
<blockquote>
<p>如果<em>a</em>和<em>d</em>是两个自然数，<em>d</em>非零，可以证明存在两个唯一的整数&nbsp;<em>q </em>和&nbsp;<em>r</em>，满足&nbsp;<em>a</em> =&nbsp;<em>qd</em> +&nbsp;<em>r</em> 且0 ≤&nbsp;<em>r</em> &lt;&nbsp;<em>d</em>。其中，<em>q </em>被称为商，<em>r </em>被称为余数。</p>
</blockquote>
<p>那么对于负数，是否可以沿用这样的定义呢？我们发现，假如我们按照正数求余的规则求 (-7) mod 3 的结果，就可以表示 -7 为 (-3)* 3 +2。其中，2是余数，-3是商。</p>
<p>那么，各种编程语言和计算器是否是按照这样理解的呢？下面是几种软件中对此的理解。<span></span></p>
<p><strong>C++（G++ 编译）</strong>： cout &lt;&lt; (-7) % 3; // 输出<strong> -1</strong></p>
<p><strong>Java（1.6）</strong>： System.out.println((-7) % 3); // 输出<strong> -1</strong></p>
<p><strong>Python 2.6</strong>：&gt;&gt;&gt; &nbsp;(-7) % 3 // 输出<strong> 2</strong></p>
<p><strong><a href="http://www.baidu.com/s?bs=%28-7%29+mod+2&amp;f=8&amp;wd=%28-7%29+mod+3"><span style="COLOR: #728f6f">百度计算器</span></a>：</strong>(-7) mod 3 =<strong> 2</strong></p>
<p><a href="http://www.google.com.hk/search?sourceid=chrome&amp;ie=UTF-8&amp;q=%28-7%29+mod+3"><strong><span style="COLOR: #728f6f">Google 计算器</span></strong></a>：(-7) mod 3 = <strong>2</strong></p>
<div><strong><a href="http://www.youdao.com/search?q=%28-7%29+%25+3&amp;ue=utf8&amp;keyfrom=web.index"><span style="COLOR: #728f6f">有道计算器</span></a>：</strong>(-7) mod 3 = -1</div>
<div>可以看到，结果特别有意思。这个问题是<strong>百家争鸣</strong>的。看来我们不能直接把正数的法则加在负数上。实际上，在<strong>整数范围</strong>内，自然数的求余法则并不被很多人所接受，大家大多认可的是下面的这个<strong>定义2</strong>。</div>
<blockquote>
<p>如果<em>a</em> 与<em>d</em> 是<strong>整数</strong>，<em>d</em> 非零，那么<strong>余数</strong> <em>r </em>满足这样的关系：</p>
<p>a = qd + r , q 为整数，且0 ≤ |r| &lt; |d|。</p>
</blockquote>
<p>可以看到，这个定义导致了有负数的求余并不是我们想象的那么简单，比如，-1 和 2 都是 (-7) mod 3 正确的结果，因为这两个数都符合定义。这种情况下，对于取模运算，可能有两个数都可以符合要求。我们把 -1 和 2 分别叫做<strong>正余数</strong>和<strong>负余数</strong>。通常，当除以d 时，如果正余数为r1，负余数为r2，那么有</p>
<blockquote>
<p>r1 = r2 + d</p>
</blockquote>
<p>对负数余数不明确的定义可能导致严重的计算问题，对于处理关键任务的系统，错误的选择会导致严重的后果。</p>
<p>看完了&nbsp;(-7) mod 3，下面我们来看一看 7 mod (-3) 的情况（看清楚，前面是 7 带负号，现在是 3 带负号）。根据<strong>定义2</strong>，7 = (-3) * (-2) + 1 或7 = (-3) * (-3) -2，所以余数为 1 或 -2。</p>
<p><strong>C++（G++ 编译）</strong>： cout &lt;&lt; 7 % (-3); // 输出<strong> 1</strong></p>
<p><strong>Java（1.6）</strong>： System.out.println(7 % (-3)); // 输出&nbsp;<strong>1</strong></p>
<p><strong>Python 2.6</strong>：&gt;&gt;&gt; &nbsp;输出<strong> -2</strong></p>
<p><strong><a href="http://www.baidu.com/s?bs=%28-7%29+mod+2&amp;f=8&amp;wd=%28-7%29+mod+3"><span style="COLOR: #728f6f">百度计算器</span></a>：</strong>7 mod (-3) =<strong> -2</strong></p>
<p><strong><a href="http://www.google.com.hk/search?sourceid=chrome&amp;ie=UTF-8&amp;q=%28-7%29+mod+3"><span style="COLOR: #728f6f">Google 计算器</span></a>： </strong>7 mod (-3) = -<strong>2</strong></p>
<div><strong><a href="http://www.youdao.com/search?q=%28-7%29+%25+3&amp;ue=utf8&amp;keyfrom=web.index"><span style="COLOR: #728f6f">有道计算器</span></a>：不支持</strong></div>
<div><strong><br></strong></div>
<div>从中我们看到几个很有意思的现象：</div>
<div>
<ol>
<li>Java 紧随 C++ 的步伐，而 Python、Google、百度步调一致。难道真是<strong>物以类聚？</strong>联想一下，Google 一直支持 Python，Python 也颇有 Web 特色的感觉，而且 Google Application Engine 也用的 Python，国内的搜索引擎也不约而同地按照 Google 的定义进行运算。
</li><li>可以推断，C++ 和 Java 通常会<strong>尽量让商更大一些</strong>。比如在 (-7) mod 3中，他们以 -2 为商，余数为 -1。在 Python 和 Google 计算器中，<strong>尽量让商更小</strong>，所以以 -3 为商。在 7 mod (-3) 中效果相同：C++ 选择了 3 作为商，Python 选择了 2 作为商。但<strong>是在正整数运算中，所有语言和计算器都遵循了尽量让商小的原则，</strong>因此 7 mod 3 结果为 1 不存在争议，不会有人说它的余数是-2。
</li><li>如果按照第二点的推断，我们测试一下 (-7) mod (-3)，结果应该是前一组语言（C++，Java）返回 2，后一组返回 -1。（请注意这只是假设）</li>
</ol>
<p>于是我做了实际测试：</p>
</div>
<p><strong>C++（G++ 编译）</strong>： cout &lt;&lt; (-7) % (-3); // 输出<strong> -1</strong></p>
<p><strong>Java（1.6）</strong>： System.out.println((-7) % (-3)); // 输出 -<strong>1</strong></p>
<p><strong>Python 2.6</strong>：&gt;&gt;&gt; &nbsp;输出<strong> -1</strong></p>
<p><strong><a href="http://www.baidu.com/s?bs=%28-7%29+mod+2&amp;f=8&amp;wd=%28-7%29+mod+3"><span style="COLOR: #728f6f">百度计算器</span></a>：-</strong>7 mod (-3) =<strong> -1</strong></p>
<p><strong><a href="http://www.google.com.hk/search?sourceid=chrome&amp;ie=UTF-8&amp;q=%28-7%29+mod+3"><span style="COLOR: #728f6f">Google 计算器</span></a>： -</strong>7 mod (-3) = -<strong>1</strong></p>
<p><strong>结果让人大跌眼镜，所有语言和计算机返回结果完全一致。</strong></p>
<div><span style="COLOR: rgb(255,0,0)"><strong>总结时间到</strong></span></div>
<div><span style="COLOR: rgb(255,0,0)"><strong><br></strong></span></div>
<div>我们由此可以总结出下面两个结论：</div>
<div>
<ul>
<li>对于任何<strong>同号</strong>的两个整数，其取余结果没有争议，所有语言的运算原则都是<strong>使商尽可能小</strong>。
</li><li>对于异号的两个整数，C++/Java语言的原则是<strong>使商尽可能大，</strong>很多新型语言和网页计算器的原则是<strong>使商尽可能小。</strong></li>
</ul>
</div>
<div>最后是<strong>拓展时间</strong>。对于<strong>实数</strong>，我们也可以定义取模运算（<strong>定义3</strong>）。</div>
<div>
<blockquote>
<div>当a 和 d 是实数，且d 非零, a 除以 d 会得到另一个实数（商），没有所谓的剩余的数。但如果要求商为一个整数，则余数的概念还是有必要的。可以证明：存在唯一的整数商 q 和唯一的实数 r 使得: a = qd + r, 0 ≤ r &lt; |d|.</div>
<div style="TEXT-ALIGN: right">（转自维基百科）</div>
</blockquote>
</div>
