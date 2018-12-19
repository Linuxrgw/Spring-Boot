# Spring-Boot
<h1>springboot(四)：thymeleaf使用详解</h1>
            <span class="meta-info">
                
                
                <span class="octicon octicon-calendar"></span> 2016/05/01
                
            </span>
        </div>
    </div>
</section>
<script>
    $(document).ready(function(){

        $('.geopattern').each(function(){
            $(this).geopattern($(this).data('pattern-id'));
        });

    });
</script>
<article class="post container" itemscope itemtype="http://schema.org/BlogPosting">

    <div class="row">

        
        <div class="col-md-9 markdown-body">

            <p>在上篇文章<a href="http://www.ityouknow.com/springboot/2016/02/03/spring-boot-web.html">springboot(二)：web综合开发</a>中简单介绍了一下thymeleaf，这篇文章将更加全面详细的介绍thymeleaf的使用。thymeleaf 是新一代的模板引擎，在spring4.0中推荐使用thymeleaf来做前端模版引擎。</p>

<h2 id="thymeleaf介绍">thymeleaf介绍</h2>

<p>简单说， Thymeleaf 是一个跟 Velocity、FreeMarker 类似的模板引擎，它可以完全替代 JSP 。相较与其他的模板引擎，它有如下三个极吸引人的特点：</p>

<ul>
  <li>
    <p>1.Thymeleaf 在有网络和无网络的环境下皆可运行，即它可以让美工在浏览器查看页面的静态效果，也可以让程序员在服务器查看带数据的动态页面效果。这是由于它支持 html 原型，然后在 html 标签里增加额外的属性来达到模板+数据的展示方式。浏览器解释 html 时会忽略未定义的标签属性，所以 thymeleaf 的模板可以静态地运行；当有数据返回到页面时，Thymeleaf 标签会动态地替换掉静态内容，使页面动态显示。</p>
  </li>
  <li>
    <p>2.Thymeleaf 开箱即用的特性。它提供标准和spring标准两种方言，可以直接套用模板实现JSTL、 OGNL表达式效果，避免每天套模板、该jstl、改标签的困扰。同时开发人员也可以扩展和创建自定义的方言。</p>
  </li>
  <li>
    <p>3.Thymeleaf 提供spring标准方言和一个与 SpringMVC 完美集成的可选模块，可以快速的实现表单绑定、属性编辑器、国际化等功能。</p>
  </li>
</ul>

<h2 id="标准表达式语法">标准表达式语法</h2>

<p>它们分为四类：</p>

<ul>
  <li>1.变量表达式</li>
  <li>2.选择或星号表达式</li>
  <li>3.文字国际化表达式</li>
  <li>4.URL表达式</li>
</ul>

<h3 id="变量表达式">变量表达式</h3>

<p>变量表达式即OGNL表达式或Spring EL表达式(在Spring术语中也叫model attributes)。如下所示：<br />
 <code class="highlighter-rouge">${session.user.name}</code></p>

<p>它们将以HTML标签的一个属性来表示：</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nt">&lt;span</span> <span class="na">th:text=</span><span class="s">"${book.author.name}"</span><span class="nt">&gt;</span>  
<span class="nt">&lt;li</span> <span class="na">th:each=</span><span class="s">"book : ${books}"</span><span class="nt">&gt;</span>  
</code></pre></div></div>

<h3 id="选择星号表达式">选择(星号)表达式</h3>

<p>选择表达式很像变量表达式，不过它们用一个预先选择的对象来代替上下文变量容器(map)来执行，如下：<br />
<code class="highlighter-rouge">   *{customer.name} </code></p>

<p>被指定的object由th:object属性定义：</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code>    <span class="nt">&lt;div</span> <span class="na">th:object=</span><span class="s">"${book}"</span><span class="nt">&gt;</span>  
      ...  
      <span class="nt">&lt;span</span> <span class="na">th:text=</span><span class="s">"*{title}"</span><span class="nt">&gt;</span>...<span class="nt">&lt;/span&gt;</span>  
      ...  
    <span class="nt">&lt;/div&gt;</span>  
</code></pre></div></div>

<h3 id="文字国际化表达式">文字国际化表达式</h3>

<p>文字国际化表达式允许我们从一个外部文件获取区域文字信息(.properties)，用Key索引Value，还可以提供一组参数(可选).</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code>    #{main.title}  
    #{message.entrycreated(${entryId})}  
</code></pre></div></div>

<p>可以在模板文件中找到这样的表达式代码：</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code>    <span class="nt">&lt;table&gt;</span>  
      ...  
      <span class="nt">&lt;th</span> <span class="na">th:text=</span><span class="s">"#{header.address.city}"</span><span class="nt">&gt;</span>...<span class="nt">&lt;/th&gt;</span>  
      <span class="nt">&lt;th</span> <span class="na">th:text=</span><span class="s">"#{header.address.country}"</span><span class="nt">&gt;</span>...<span class="nt">&lt;/th&gt;</span>  
      ...  
    <span class="nt">&lt;/table&gt;</span>  
</code></pre></div></div>

<h3 id="url表达式">URL表达式</h3>

<p>URL表达式指的是把一个有用的上下文或回话信息添加到URL，这个过程经常被叫做URL重写。<br />
<code class="highlighter-rouge">    @{/order/list} </code><br />
URL还可以设置参数：<br />
<code class="highlighter-rouge">   @{/order/details(id=${orderId})} </code> <br />
相对路径：<br />
<code class="highlighter-rouge">     @{../documents/report}  </code></p>

<p>让我们看这些表达式：</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code>    <span class="nt">&lt;form</span> <span class="na">th:action=</span><span class="s">"@{/createOrder}"</span><span class="nt">&gt;</span>  
    <span class="nt">&lt;a</span> <span class="na">href=</span><span class="s">"main.html"</span> <span class="na">th:href=</span><span class="s">"@{/main}"</span><span class="nt">&gt;</span>
</code></pre></div></div>

<h3 id="变量表达式和星号表达有什么区别吗">变量表达式和星号表达有什么区别吗？</h3>

<p>如果不考虑上下文的情况下，两者没有区别；星号语法评估在选定对象上表达，而不是整个上下文 <br />
什么是选定对象？就是父标签的值，如下：</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code>  <span class="nt">&lt;div</span> <span class="na">th:object=</span><span class="s">"${session.user}"</span><span class="nt">&gt;</span>
    <span class="nt">&lt;p&gt;</span>Name: <span class="nt">&lt;span</span> <span class="na">th:text=</span><span class="s">"*{firstName}"</span><span class="nt">&gt;</span>Sebastian<span class="nt">&lt;/span&gt;</span>.<span class="nt">&lt;/p&gt;</span>
    <span class="nt">&lt;p&gt;</span>Surname: <span class="nt">&lt;span</span> <span class="na">th:text=</span><span class="s">"*{lastName}"</span><span class="nt">&gt;</span>Pepper<span class="nt">&lt;/span&gt;</span>.<span class="nt">&lt;/p&gt;</span>
    <span class="nt">&lt;p&gt;</span>Nationality: <span class="nt">&lt;span</span> <span class="na">th:text=</span><span class="s">"*{nationality}"</span><span class="nt">&gt;</span>Saturn<span class="nt">&lt;/span&gt;</span>.<span class="nt">&lt;/p&gt;</span>
  <span class="nt">&lt;/div&gt;</span>
</code></pre></div></div>

<p>这是完全等价于：</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code>  <span class="nt">&lt;div</span> <span class="na">th:object=</span><span class="s">"${session.user}"</span><span class="nt">&gt;</span>
	  <span class="nt">&lt;p&gt;</span>Name: <span class="nt">&lt;span</span> <span class="na">th:text=</span><span class="s">"${session.user.firstName}"</span><span class="nt">&gt;</span>Sebastian<span class="nt">&lt;/span&gt;</span>.<span class="nt">&lt;/p&gt;</span>
	  <span class="nt">&lt;p&gt;</span>Surname: <span class="nt">&lt;span</span> <span class="na">th:text=</span><span class="s">"${session.user.lastName}"</span><span class="nt">&gt;</span>Pepper<span class="nt">&lt;/span&gt;</span>.<span class="nt">&lt;/p&gt;</span>
	  <span class="nt">&lt;p&gt;</span>Nationality: <span class="nt">&lt;span</span> <span class="na">th:text=</span><span class="s">"${session.user.nationality}"</span><span class="nt">&gt;</span>Saturn<span class="nt">&lt;/span&gt;</span>.<span class="nt">&lt;/p&gt;</span>
  <span class="nt">&lt;/div&gt;</span>
</code></pre></div></div>

<p>当然，美元符号和星号语法可以混合使用：</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code>  <span class="nt">&lt;div</span> <span class="na">th:object=</span><span class="s">"${session.user}"</span><span class="nt">&gt;</span>
	  <span class="nt">&lt;p&gt;</span>Name: <span class="nt">&lt;span</span> <span class="na">th:text=</span><span class="s">"*{firstName}"</span><span class="nt">&gt;</span>Sebastian<span class="nt">&lt;/span&gt;</span>.<span class="nt">&lt;/p&gt;</span>
  	  <span class="nt">&lt;p&gt;</span>Surname: <span class="nt">&lt;span</span> <span class="na">th:text=</span><span class="s">"${session.user.lastName}"</span><span class="nt">&gt;</span>Pepper<span class="nt">&lt;/span&gt;</span>.<span class="nt">&lt;/p&gt;</span>
      <span class="nt">&lt;p&gt;</span>Nationality: <span class="nt">&lt;span</span> <span class="na">th:text=</span><span class="s">"*{nationality}"</span><span class="nt">&gt;</span>Saturn<span class="nt">&lt;/span&gt;</span>.<span class="nt">&lt;/p&gt;</span>
  <span class="nt">&lt;/div&gt;</span>
</code></pre></div></div>

<h3 id="表达式支持的语法">表达式支持的语法</h3>

<h4 id="字面literals">字面（Literals）</h4>

<ul>
  <li>文本文字（Text literals）: <code class="highlighter-rouge">'one text', 'Another one!',…</code></li>
  <li>数字文本（Number literals）: <code class="highlighter-rouge">0, 34, 3.0, 12.3,…</code></li>
  <li>布尔文本（Boolean literals）: <code class="highlighter-rouge">true, false</code></li>
  <li>空（Null literal）: <code class="highlighter-rouge">null</code></li>
  <li>文字标记（Literal tokens）: <code class="highlighter-rouge">one, sometext, main,…</code></li>
</ul>

<h4 id="文本操作text-operations">文本操作（Text operations）</h4>
<ul>
  <li>字符串连接(String concatenation): <code class="highlighter-rouge">+</code></li>
  <li>文本替换（Literal substitutions）: <code class="highlighter-rouge">|The name is ${name}|</code></li>
</ul>

<h4 id="算术运算arithmetic-operations">算术运算（Arithmetic operations）</h4>
<ul>
  <li>二元运算符（Binary operators）: <code class="highlighter-rouge">+, -, *, /, %</code></li>
  <li>减号（单目运算符）Minus sign (unary operator): <code class="highlighter-rouge">-</code></li>
</ul>

<h4 id="布尔操作boolean-operations">布尔操作（Boolean operations）</h4>
<ul>
  <li>二元运算符（Binary operators）:<code class="highlighter-rouge">and, or</code></li>
  <li>布尔否定（一元运算符）Boolean negation (unary operator):<code class="highlighter-rouge">!, not</code></li>
</ul>

<h4 id="比较和等价comparisons-and-equality">比较和等价(Comparisons and equality)</h4>
<ul>
  <li>比较（Comparators）: <code class="highlighter-rouge">&gt;, &lt;, &gt;=, &lt;= (gt, lt, ge, le)</code></li>
  <li>等值运算符（Equality operators）:<code class="highlighter-rouge">==, != (eq, ne)</code></li>
</ul>

<h4 id="条件运算符conditional-operators">条件运算符（Conditional operators）</h4>
<ul>
  <li>If-then: <code class="highlighter-rouge">(if) ? (then)</code></li>
  <li>If-then-else: <code class="highlighter-rouge">(if) ? (then) : (else)</code></li>
  <li>Default: (value) ?: <code class="highlighter-rouge">(defaultvalue)</code></li>
</ul>

<p>所有这些特征可以被组合并嵌套：</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code>'User is of type ' + (${user.isAdmin()} ? 'Administrator' : (${user.type} ?: 'Unknown'))
</code></pre></div></div>

<h2 id="常用th标签都有那些">常用th标签都有那些？</h2>

<table>
  <thead>
    <tr>
      <th>关键字</th>
      <th>功能介绍</th>
      <th>案例</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>th:id</td>
      <td>替换id</td>
      <td><code class="highlighter-rouge">    &lt;input th:id="'xxx' + ${collect.id}"/&gt; </code></td>
    </tr>
    <tr>
      <td>th:text</td>
      <td>文本替换</td>
      <td><code class="highlighter-rouge">&lt;p  th:text="${collect.description}"&gt;description&lt;/p&gt;</code></td>
    </tr>
    <tr>
      <td>th:utext</td>
      <td>支持html的文本替换</td>
      <td><code class="highlighter-rouge">&lt;p  th:utext="${htmlcontent}"&gt;conten&lt;/p&gt;</code></td>
    </tr>
    <tr>
      <td>th:object</td>
      <td>替换对象</td>
      <td><code class="highlighter-rouge">&lt;div th:object="${session.user}"&gt; </code></td>
    </tr>
    <tr>
      <td>th:value</td>
      <td>属性赋值</td>
      <td><code class="highlighter-rouge">&lt;input th:value="${user.name}" /&gt; </code></td>
    </tr>
    <tr>
      <td>th:with</td>
      <td>变量赋值运算</td>
      <td><code class="highlighter-rouge">&lt;div th:with="isEven=${prodStat.count}%2==0"&gt;&lt;/div&gt; </code></td>
    </tr>
    <tr>
      <td>th:style</td>
      <td>设置样式</td>
      <td><code class="highlighter-rouge">th:style="'display:' + @{(${sitrue} ? 'none' : 'inline-block')} + ''" </code></td>
    </tr>
    <tr>
      <td>th:onclick</td>
      <td>点击事件</td>
      <td><code class="highlighter-rouge">th:onclick="'getCollect()'" </code></td>
    </tr>
    <tr>
      <td>th:each</td>
      <td>属性赋值</td>
      <td><code class="highlighter-rouge">tr th:each="user,userStat:${users}"&gt; </code></td>
    </tr>
    <tr>
      <td>th:if</td>
      <td>判断条件</td>
      <td><code class="highlighter-rouge"> &lt;a th:if="${userId == collect.userId}" &gt; </code></td>
    </tr>
    <tr>
      <td>th:unless</td>
      <td>和th:if判断相反</td>
      <td><code class="highlighter-rouge">&lt;a th:href="@{/login}" th:unless=${session.user != null}&gt;Login&lt;/a&gt; </code></td>
    </tr>
    <tr>
      <td>th:href</td>
      <td>链接地址</td>
      <td><code class="highlighter-rouge">&lt;a th:href="@{/login}" th:unless=${session.user != null}&gt;Login&lt;/a&gt; /&gt; </code></td>
    </tr>
    <tr>
      <td>th:switch</td>
      <td>多路选择 配合th:case 使用</td>
      <td><code class="highlighter-rouge">&lt;div th:switch="${user.role}"&gt; </code></td>
    </tr>
    <tr>
      <td>th:case</td>
      <td>th:switch的一个分支</td>
      <td><code class="highlighter-rouge"> &lt;p th:case="'admin'"&gt;User is an administrator&lt;/p&gt;</code></td>
    </tr>
    <tr>
      <td>th:fragment</td>
      <td>布局标签，定义一个代码片段，方便其它地方引用</td>
      <td><code class="highlighter-rouge">&lt;div th:fragment="alert"&gt;</code></td>
    </tr>
    <tr>
      <td>th:include</td>
      <td>布局标签，替换内容到引入的文件</td>
      <td><code class="highlighter-rouge">&lt;head th:include="layout :: htmlhead" th:with="title='xx'"&gt;&lt;/head&gt; /&gt; </code></td>
    </tr>
    <tr>
      <td>th:replace</td>
      <td>布局标签，替换整个标签到引入的文件</td>
      <td><code class="highlighter-rouge">&lt;div th:replace="fragments/header :: title"&gt;&lt;/div&gt; </code></td>
    </tr>
    <tr>
      <td>th:selected</td>
      <td>selected选择框 选中</td>
      <td><code class="highlighter-rouge">th:selected="(${xxx.id} == ${configObj.dd})"</code></td>
    </tr>
    <tr>
      <td>th:src</td>
      <td>图片类地址引入</td>
      <td><code class="highlighter-rouge">&lt;img class="img-responsive" alt="App Logo" th:src="@{/img/logo.png}"  /&gt; </code></td>
    </tr>
    <tr>
      <td>th:inline</td>
      <td>定义js脚本可以使用变量</td>
      <td><code class="highlighter-rouge">&lt;script type="text/javascript" th:inline="javascript"&gt;</code></td>
    </tr>
    <tr>
      <td>th:action</td>
      <td>表单提交的地址</td>
      <td><code class="highlighter-rouge">&lt;form action="subscribe.html" th:action="@{/subscribe}"&gt;</code></td>
    </tr>
    <tr>
      <td>th:remove</td>
      <td>删除某个属性</td>
      <td><code class="highlighter-rouge">&lt;tr th:remove="all"&gt;   1.all:删除包含标签和所有的孩子。2.body:不包含标记删除,但删除其所有的孩子。3.tag:包含标记的删除,但不删除它的孩子。4.all-but-first:删除所有包含标签的孩子,除了第一个。5.none:什么也不做。这个值是有用的动态评估。</code></td>
    </tr>
    <tr>
      <td>th:attr</td>
      <td>设置标签属性，多个属性可以用逗号分隔</td>
      <td>比如 <code class="highlighter-rouge">th:attr="src=@{/image/aa.jpg},title=#{logo}"</code>，此标签不太优雅，一般用的比较少。</td>
    </tr>
  </tbody>
</table>

<p>还有非常多的标签，这里只列出最常用的几个,由于一个标签内可以包含多个th:x属性，其生效的优先级顺序为:
<code class="highlighter-rouge">include,each,if/unless/switch/case,with,attr/attrprepend/attrappend,value/href,src ,etc,text/utext,fragment,remove。 </code></p>

<h2 id="几种常用的使用方法">几种常用的使用方法</h2>

<h3 id="1赋值字符串拼接">1、赋值、字符串拼接</h3>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code> <span class="nt">&lt;p</span>  <span class="na">th:text=</span><span class="s">"${collect.description}"</span><span class="nt">&gt;</span>description<span class="nt">&lt;/p&gt;</span>
 <span class="nt">&lt;span</span> <span class="na">th:text=</span><span class="s">"'Welcome to our application, ' + ${user.name} + '!'"</span><span class="nt">&gt;</span>
</code></pre></div></div>
<p>字符串拼接还有另外一种简洁的写法</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nt">&lt;span</span> <span class="na">th:text=</span><span class="s">"|Welcome to our application, ${user.name}!|"</span><span class="nt">&gt;</span>
</code></pre></div></div>

<h3 id="2条件判断-ifunless">2、条件判断 If/Unless</h3>

<p>Thymeleaf中使用th:if和th:unless属性进行条件判断，下面的例子中，<code class="highlighter-rouge">&lt;a&gt;</code>标签只有在<code class="highlighter-rouge">th:if</code>中条件成立时才显示：</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nt">&lt;a</span> <span class="na">th:if=</span><span class="s">"${myself=='yes'}"</span> <span class="nt">&gt;</span> <span class="nt">&lt;/i&gt;</span> <span class="nt">&lt;/a&gt;</span>
<span class="nt">&lt;a</span> <span class="na">th:unless=</span><span class="s">${session.user</span> <span class="err">!=</span> <span class="na">null</span><span class="err">}</span> <span class="na">th:href=</span><span class="s">"@{/login}"</span> <span class="nt">&gt;</span>Login<span class="nt">&lt;/a&gt;</span>
</code></pre></div></div>
<p>th:unless于th:if恰好相反，只有表达式中的条件不成立，才会显示其内容。</p>

<p>也可以使用  <code class="highlighter-rouge">(if) ? (then) : (else)</code> 这种语法来判断显示的内容</p>

<h3 id="3for-循环">3、for 循环</h3>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code>  <span class="nt">&lt;tr</span>  <span class="na">th:each=</span><span class="s">"collect,iterStat : ${collects}"</span><span class="nt">&gt;</span> 
     <span class="nt">&lt;th</span> <span class="na">scope=</span><span class="s">"row"</span> <span class="na">th:text=</span><span class="s">"${collect.id}"</span><span class="nt">&gt;</span>1<span class="nt">&lt;/th&gt;</span>
     <span class="nt">&lt;td</span> <span class="nt">&gt;</span>
        <span class="nt">&lt;img</span> <span class="na">th:src=</span><span class="s">"${collect.webLogo}"</span><span class="nt">/&gt;</span>
     <span class="nt">&lt;/td&gt;</span>
     <span class="nt">&lt;td</span> <span class="na">th:text=</span><span class="s">"${collect.url}"</span><span class="nt">&gt;</span>Mark<span class="nt">&lt;/td&gt;</span>
     <span class="nt">&lt;td</span> <span class="na">th:text=</span><span class="s">"${collect.title}"</span><span class="nt">&gt;</span>Otto<span class="nt">&lt;/td&gt;</span>
     <span class="nt">&lt;td</span> <span class="na">th:text=</span><span class="s">"${collect.description}"</span><span class="nt">&gt;</span>@mdo<span class="nt">&lt;/td&gt;</span>
     <span class="nt">&lt;td</span> <span class="na">th:text=</span><span class="s">"${terStat.index}"</span><span class="nt">&gt;</span>index<span class="nt">&lt;/td&gt;</span>
 <span class="nt">&lt;/tr&gt;</span>
</code></pre></div></div>

<p>iterStat称作状态变量，属性有：</p>

<ul>
  <li>index:当前迭代对象的index（从0开始计算）</li>
  <li>count: 当前迭代对象的index(从1开始计算)</li>
  <li>size:被迭代对象的大小</li>
  <li>current:当前迭代变量</li>
  <li>even/odd:布尔值，当前循环是否是偶数/奇数（从0开始计算）</li>
  <li>first:布尔值，当前循环是否是第一个</li>
  <li>last:布尔值，当前循环是否是最后一个</li>
</ul>

<h3 id="4url">4、URL</h3>

<p>URL在Web应用模板中占据着十分重要的地位，需要特别注意的是Thymeleaf对于URL的处理是通过语法@{…}来处理的。
如果需要Thymeleaf对URL进行渲染，那么务必使用th:href，th:src等属性，下面是一个例子</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="c">&lt;!-- Will produce 'http://localhost:8080/standard/unread' (plus rewriting) --&gt;</span>
 <span class="nt">&lt;a</span>  <span class="na">th:href=</span><span class="s">"@{/standard/{type}(type=${type})}"</span><span class="nt">&gt;</span>view<span class="nt">&lt;/a&gt;</span>

<span class="c">&lt;!-- Will produce '/gtvg/order/3/details' (plus rewriting) --&gt;</span>
<span class="nt">&lt;a</span> <span class="na">href=</span><span class="s">"details.html"</span> <span class="na">th:href=</span><span class="s">"@{/order/{orderId}/details(orderId=${o.id})}"</span><span class="nt">&gt;</span>view<span class="nt">&lt;/a&gt;</span>
</code></pre></div></div>
<p>设置背景</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nt">&lt;div</span> <span class="na">th:style=</span><span class="s">"'background:url(' + @{/&lt;path-to-image&gt;} + ');'"</span><span class="nt">&gt;&lt;/div&gt;</span>
</code></pre></div></div>

<p>根据属性值改变背景</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code> <span class="nt">&lt;div</span> <span class="na">class=</span><span class="s">"media-object resource-card-image"</span>  <span class="na">th:style=</span><span class="s">"'background:url(' + @{(${collect.webLogo}=='' ? 'img/favicon.png' : ${collect.webLogo})} + ')'"</span> <span class="nt">&gt;&lt;/div&gt;</span>
</code></pre></div></div>
<p>几点说明：</p>

<ul>
  <li>上例中URL最后的<code class="highlighter-rouge">(orderId=${o.id})</code> 表示将括号内的内容作为URL参数处理，该语法避免使用字符串拼接，大大提高了可读性</li>
  <li><code class="highlighter-rouge">@{...}</code>表达式中可以通过<code class="highlighter-rouge">{orderId}</code>访问Context中的orderId变量</li>
  <li><code class="highlighter-rouge">@{/order}</code>是Context相关的相对路径，在渲染时会自动添加上当前Web应用的Context名字，假设context名字为app，那么结果应该是/app/order</li>
</ul>

<h3 id="5内联js">5、内联js</h3>

<p>内联文本：[[…]]内联文本的表示方式，使用时，必须先用th:inline=”text/javascript/none”激活，th:inline可以在父级标签内使用，甚至作为body的标签。内联文本尽管比th:text的代码少，不利于原型显示。</p>

<div class="language-js highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="o">&lt;</span><span class="nx">script</span> <span class="nx">th</span><span class="p">:</span><span class="nx">inline</span><span class="o">=</span><span class="s2">"javascript"</span><span class="o">&gt;</span>
<span class="cm">/*&lt;![CDATA[*/</span>
<span class="p">...</span>
<span class="kd">var</span> <span class="nx">username</span> <span class="o">=</span> <span class="cm">/*[[${sesion.user.name}]]*/</span> <span class="s1">'Sebastian'</span><span class="p">;</span>
<span class="kd">var</span> <span class="nx">size</span> <span class="o">=</span> <span class="cm">/*[[${size}]]*/</span> <span class="mi">0</span><span class="p">;</span>
<span class="p">...</span>
<span class="cm">/*]]&gt;*/</span>
<span class="o">&lt;</span><span class="sr">/script</span><span class="err">&gt;
</span></code></pre></div></div>

<p>js附加代码：</p>

<div class="language-js highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="cm">/*[+
var msg = 'This is a working application';
+]*/</span>
</code></pre></div></div>

<p>js移除代码：</p>

<div class="language-js highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="cm">/*[- */</span>
<span class="kd">var</span> <span class="nx">msg</span> <span class="o">=</span> <span class="s1">'This is a non-working template'</span><span class="p">;</span>
<span class="cm">/* -]*/</span>
</code></pre></div></div>

<h3 id="6内嵌变量">6、内嵌变量</h3>

<p>为了模板更加易用，Thymeleaf还提供了一系列Utility对象（内置于Context中），可以通过#直接访问：</p>

<ul>
  <li>dates ：  <em>java.util.Date的功能方法类。</em></li>
  <li>calendars :  <em>类似#dates，面向java.util.Calendar</em></li>
  <li>numbers :  <em>格式化数字的功能方法类</em></li>
  <li>strings :  <em>字符串对象的功能类，contains,startWiths,prepending/appending等等。</em></li>
  <li>objects:  <em>对objects的功能类操作。</em></li>
  <li>bools:  <em>对布尔值求值的功能方法。</em></li>
  <li>arrays：<em>对数组的功能类方法。</em></li>
  <li>lists:   <em>对lists功能类方法</em></li>
  <li>sets</li>
  <li>maps<br />
 …</li>
</ul>

<p>下面用一段代码来举例一些常用的方法：</p>

<h4 id="dates">dates</h4>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code>/*
 * Format date with the specified pattern
 * Also works with arrays, lists or sets
 */
${#dates.format(date, 'dd/MMM/yyyy HH:mm')}
${#dates.arrayFormat(datesArray, 'dd/MMM/yyyy HH:mm')}
${#dates.listFormat(datesList, 'dd/MMM/yyyy HH:mm')}
${#dates.setFormat(datesSet, 'dd/MMM/yyyy HH:mm')}

/*
 * Create a date (java.util.Date) object for the current date and time
 */
${#dates.createNow()}

/*
 * Create a date (java.util.Date) object for the current date (time set to 00:00)
 */
${#dates.createToday()}
</code></pre></div></div>

<h4 id="strings">strings</h4>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code>/*
 * Check whether a String is empty (or null). Performs a trim() operation before check
 * Also works with arrays, lists or sets
 */
${#strings.isEmpty(name)}
${#strings.arrayIsEmpty(nameArr)}
${#strings.listIsEmpty(nameList)}
${#strings.setIsEmpty(nameSet)}

/*
 * Check whether a String starts or ends with a fragment
 * Also works with arrays, lists or sets
 */
${#strings.startsWith(name,'Don')}                  // also array*, list* and set*
${#strings.endsWith(name,endingFragment)}           // also array*, list* and set*

/*
 * Compute length
 * Also works with arrays, lists or sets
 */
${#strings.length(str)}

/*
 * Null-safe comparison and concatenation
 */
${#strings.equals(str)}
${#strings.equalsIgnoreCase(str)}
${#strings.concat(str)}
${#strings.concatReplaceNulls(str)}

/*
 * Random
 */
${#strings.randomAlphanumeric(count)}

</code></pre></div></div>

<h2 id="使用thymeleaf布局">使用thymeleaf布局</h2>

<p>使用thymeleaf布局非常的方便</p>

<p>定义代码片段</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nt">&lt;footer</span> <span class="na">th:fragment=</span><span class="s">"copy"</span><span class="nt">&gt;</span> 
<span class="ni">&amp;copy;</span> 2016
<span class="nt">&lt;/footer&gt;</span>
</code></pre></div></div>

<p>在页面任何地方引入：</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nt">&lt;body&gt;</span> 
  <span class="nt">&lt;div</span> <span class="na">th:include=</span><span class="s">"footer :: copy"</span><span class="nt">&gt;&lt;/div&gt;</span>
  <span class="nt">&lt;div</span> <span class="na">th:replace=</span><span class="s">"footer :: copy"</span><span class="nt">&gt;&lt;/div&gt;</span>
 <span class="nt">&lt;/body&gt;</span>
</code></pre></div></div>

<p>th:include 和 th:replace区别，include只是加载，replace是替换</p>

<p>返回的HTML如下：</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nt">&lt;body&gt;</span> 
   <span class="nt">&lt;div&gt;</span> <span class="ni">&amp;copy;</span> 2016 <span class="nt">&lt;/div&gt;</span> 
  <span class="nt">&lt;footer&gt;</span><span class="ni">&amp;copy;</span> 2016 <span class="nt">&lt;/footer&gt;</span> 
<span class="nt">&lt;/body&gt;</span>
</code></pre></div></div>

<p>下面是一个常用的后台页面布局，将整个页面分为头部，尾部、菜单栏、隐藏栏，点击菜单只改变content区域的页面</p>
<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nt">&lt;body</span> <span class="na">class=</span><span class="s">"layout-fixed"</span><span class="nt">&gt;</span>
  <span class="nt">&lt;div</span> <span class="na">th:fragment=</span><span class="s">"navbar"</span>  <span class="na">class=</span><span class="s">"wrapper"</span>  <span class="na">role=</span><span class="s">"navigation"</span><span class="nt">&gt;</span>
	<span class="nt">&lt;div</span> <span class="na">th:replace=</span><span class="s">"fragments/header :: header"</span><span class="nt">&gt;</span>Header<span class="nt">&lt;/div&gt;</span>
	<span class="nt">&lt;div</span> <span class="na">th:replace=</span><span class="s">"fragments/left :: left"</span><span class="nt">&gt;</span>left<span class="nt">&lt;/div&gt;</span>
	<span class="nt">&lt;div</span> <span class="na">th:replace=</span><span class="s">"fragments/sidebar :: sidebar"</span><span class="nt">&gt;</span>sidebar<span class="nt">&lt;/div&gt;</span>
	<span class="nt">&lt;div</span> <span class="na">layout:fragment=</span><span class="s">"content"</span> <span class="na">id=</span><span class="s">"content"</span> <span class="nt">&gt;&lt;/div&gt;</span>
	<span class="nt">&lt;div</span> <span class="na">th:replace=</span><span class="s">"fragments/footer :: footer"</span><span class="nt">&gt;</span>footer<span class="nt">&lt;/div&gt;</span>
  <span class="nt">&lt;/div&gt;</span>
<span class="nt">&lt;/body&gt;</span>
</code></pre></div></div>

<p>任何页面想使用这样的布局值只需要替换中见的 content模块即可</p>
<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code> <span class="nt">&lt;html</span> <span class="na">xmlns:th=</span><span class="s">"http://www.thymeleaf.org"</span> <span class="na">layout:decorator=</span><span class="s">"layout"</span><span class="nt">&gt;</span>
   <span class="nt">&lt;body&gt;</span>
      <span class="nt">&lt;section</span> <span class="na">layout:fragment=</span><span class="s">"content"</span><span class="nt">&gt;</span>
    ...

</code></pre></div></div>

<p>也可以在引用模版的时候传参</p>

<div class="language-html highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nt">&lt;head</span> <span class="na">th:include=</span><span class="s">"layout :: htmlhead"</span> <span class="na">th:with=</span><span class="s">"title='Hello'"</span><span class="nt">&gt;&lt;/head&gt;</span>
</code></pre></div></div>

<p>layout 是文件地址，如果有文件夹可以这样写  fileName/layout:htmlhead<br />
htmlhead 是指定义的代码片段 如 <code class="highlighter-rouge">th:fragment="copy"</code></p>

<h2 id="源码案例">源码案例</h2>

<p>这里有一个开源项目几乎使用了这里介绍的所有标签和布局，大家可以参考：</p>

<p><strong><a href="https://github.com/ityouknow/spring-boot-examples">示例代码-github</a></strong></p>

<p><strong><a href="https://gitee.com/ityouknow/spring-boot-examples">示例代码-码云</a></strong></p>

<h2 id="参考">参考</h2>

<p><a href="http://www.thymeleaf.org/doc/tutorials/2.1/thymeleafspring.html#integrating-thymeleaf-with-spring">thymeleaf官方指南</a><br />
<a href="http://www.tianmaying.com/tutorial/using-thymeleaf">新一代Java模板引擎Thymeleaf</a><br />
<a href="http://www.webinno.cn/blog/article/view/131">Thymeleaf基本知识</a> <br />
<a href="http://v8en.com/news/list/47/0">thymeleaf总结文章</a><br />
<a href="http://www.cnblogs.com/lazio10000/p/5603955.html">Thymeleaf 模板的使用</a><br />
<a href="http://www.blogjava.net/bjwulin/archive/2013/02/07/395234.html">thymeleaf 学习笔记</a></p>

<hr />

<p><strong>作者：纯洁的微笑</strong><br />
<strong>出处：<a href="http://www.ityouknow.com">www.ityouknow.com</a></strong> <br />
