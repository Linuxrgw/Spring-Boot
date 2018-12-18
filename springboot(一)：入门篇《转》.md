# Spring-Boot
 <h2 id="什么是spring-boot">什么是spring boot</h2>

<p>Spring Boot是由Pivotal团队提供的全新框架，其设计目的是用来简化新Spring应用的初始搭建以及开发过程。该框架使用了特定的方式来进行配置，从而使开发人员不再需要定义样板化的配置。用我的话来理解，就是spring boot其实不是什么新的框架，它默认配置了很多框架的使用方式，就像maven整合了所有的jar包，spring boot整合了所有的框架（不知道这样比喻是否合适）。</p>

<h2 id="使用spring-boot有什么好处">使用spring boot有什么好处</h2>

<p>其实就是简单、快速、方便！平时如果我们需要搭建一个spring web项目的时候需要怎么做呢？</p>

<ul>
  <li>1）配置web.xml，加载spring和spring mvc</li>
  <li>2）配置数据库连接、配置spring事务</li>
  <li>3）配置加载配置文件的读取，开启注解</li>
  <li>4）配置日志文件</li>
  <li>…</li>
  <li>配置完成之后部署tomcat 调试</li>
  <li>…</li>
</ul>

<p>现在非常流行微服务，如果我这个项目仅仅只是需要发送一个邮件，如果我的项目仅仅是生产一个积分；我都需要这样折腾一遍!</p>

<p>但是如果使用spring boot呢？<br />
很简单，我仅仅只需要非常少的几个配置就可以迅速方便的搭建起来一套web项目或者是构建一个微服务！</p>

<p>使用sping boot到底有多爽，用下面这幅图来表达</p>

<p><img src="http://www.itmind.net/assets/images/2016/dog.jpg" alt="" /></p>

<h2 id="快速入门">快速入门</h2>

<p>说了那么多，手痒痒的很，马上来一发试试!</p>

<p><strong>maven构建项目</strong></p>

<ul>
  <li>1、访问http://start.spring.io/</li>
  <li>2、选择构建工具Maven Project、Spring Boot版本1.3.6以及一些工程基本信息，点击“Switch to the full version.”java版本选择1.7，可参考下图所示：</li>
</ul>

<p><img src="http://www.itmind.net/assets/images/2016/springboot1.png" alt="" /></p>

<ul>
  <li>3、点击Generate Project下载项目压缩包</li>
  <li>4、解压后，使用eclipse，Import -&gt; Existing Maven Projects -&gt; Next -&gt;选择解压后的文件夹-&gt; Finsh，OK done!</li>
</ul>

<p><strong>项目结构介绍</strong></p>

<p><img src="http://www.itmind.net/assets/images/2016/springboot2.png" alt="" /></p>

<p>如上图所示，Spring Boot的基础结构共三个文件:</p>
<ul>
  <li>src/main/java  程序开发以及主程序入口</li>
  <li>src/main/resources 配置文件</li>
  <li>src/test/java  测试程序</li>
</ul>

<p>另外，spingboot建议的目录结果如下：<br />
root package结构：<code class="highlighter-rouge">com.example.myproject</code></p>

<div class="language-java highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="n">com</span>
  <span class="o">+-</span> <span class="n">example</span>
    <span class="o">+-</span> <span class="n">myproject</span>
      <span class="o">+-</span> <span class="n">Application</span><span class="o">.</span><span class="na">java</span>
      <span class="o">|</span>
      <span class="o">+-</span> <span class="n">domain</span>
      <span class="o">|</span>  <span class="o">+-</span> <span class="n">Customer</span><span class="o">.</span><span class="na">java</span>
      <span class="o">|</span>  <span class="o">+-</span> <span class="n">CustomerRepository</span><span class="o">.</span><span class="na">java</span>
      <span class="o">|</span>
      <span class="o">+-</span> <span class="n">service</span>
      <span class="o">|</span>  <span class="o">+-</span> <span class="n">CustomerService</span><span class="o">.</span><span class="na">java</span>
      <span class="o">|</span>
      <span class="o">+-</span> <span class="n">controller</span>
      <span class="o">|</span>  <span class="o">+-</span> <span class="n">CustomerController</span><span class="o">.</span><span class="na">java</span>
      <span class="o">|</span>
</code></pre></div></div>

<ul>
  <li>1、Application.java 建议放到根目录下面,主要用于做一些框架配置</li>
  <li>2、domain目录主要用于实体（Entity）与数据访问层（Repository）</li>
  <li>3、service 层主要是业务类代码</li>
  <li>4、controller 负责页面访问控制</li>
</ul>

<p>采用默认配置可以省去很多配置，当然也可以根据自己的喜欢来进行更改<br />
最后，启动Application main方法，至此一个java项目搭建好了！</p>

<p><strong>引入web模块</strong></p>

<p>1、pom.xml中添加支持web的模块：</p>

<div class="language-xml highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nt">&lt;dependency&gt;</span>
        <span class="nt">&lt;groupId&gt;</span>org.springframework.boot<span class="nt">&lt;/groupId&gt;</span>
        <span class="nt">&lt;artifactId&gt;</span>spring-boot-starter-web<span class="nt">&lt;/artifactId&gt;</span>
 <span class="nt">&lt;/dependency&gt;</span>
</code></pre></div></div>

<p>pom.xml文件中默认有两个模块：</p>

<p><code class="highlighter-rouge">spring-boot-starter</code> ：核心模块，包括自动配置支持、日志和YAML；</p>

<p><code class="highlighter-rouge">spring-boot-starter-test</code> ：测试模块，包括JUnit、Hamcrest、Mockito。</p>

<p>2、编写controller内容：</p>

<div class="language-java highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nd">@RestController</span>
<span class="kd">public</span> <span class="kd">class</span> <span class="nc">HelloWorldController</span> <span class="o">{</span>
    <span class="nd">@RequestMapping</span><span class="o">(</span><span class="s">"/hello"</span><span class="o">)</span>
    <span class="kd">public</span> <span class="n">String</span> <span class="nf">index</span><span class="o">()</span> <span class="o">{</span>
        <span class="k">return</span> <span class="s">"Hello World"</span><span class="o">;</span>
    <span class="o">}</span>
<span class="o">}</span>
</code></pre></div></div>

<p><code class="highlighter-rouge">@RestController</code> 的意思就是controller里面的方法都以json格式输出，不用再写什么jackjson配置的了！</p>

<p>3、启动主程序，打开浏览器访问http://localhost:8080/hello，就可以看到效果了，有木有很简单！</p>

<p><strong>如何做单元测试</strong></p>

<p>打开的src/test/下的测试入口，编写简单的http请求来测试；使用mockmvc进行，利用MockMvcResultHandlers.print()打印出执行结果。</p>

<div class="language-java highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nd">@RunWith</span><span class="o">(</span><span class="n">SpringRunner</span><span class="o">.</span><span class="na">class</span><span class="o">)</span>
<span class="nd">@SpringBootTest</span>
<span class="kd">public</span> <span class="kd">class</span> <span class="nc">HelloTests</span> <span class="o">{</span>

  
    <span class="kd">private</span> <span class="n">MockMvc</span> <span class="n">mvc</span><span class="o">;</span>

    <span class="nd">@Before</span>
    <span class="kd">public</span> <span class="kt">void</span> <span class="nf">setUp</span><span class="o">()</span> <span class="kd">throws</span> <span class="n">Exception</span> <span class="o">{</span>
        <span class="n">mvc</span> <span class="o">=</span> <span class="n">MockMvcBuilders</span><span class="o">.</span><span class="na">standaloneSetup</span><span class="o">(</span><span class="k">new</span> <span class="n">HelloWorldController</span><span class="o">()).</span><span class="na">build</span><span class="o">();</span>
    <span class="o">}</span>

    <span class="nd">@Test</span>
    <span class="kd">public</span> <span class="kt">void</span> <span class="nf">getHello</span><span class="o">()</span> <span class="kd">throws</span> <span class="n">Exception</span> <span class="o">{</span>
        <span class="n">mvc</span><span class="o">.</span><span class="na">perform</span><span class="o">(</span><span class="n">MockMvcRequestBuilders</span><span class="o">.</span><span class="na">get</span><span class="o">(</span><span class="s">"/hello"</span><span class="o">).</span><span class="na">accept</span><span class="o">(</span><span class="n">MediaType</span><span class="o">.</span><span class="na">APPLICATION_JSON</span><span class="o">))</span>
                <span class="o">.</span><span class="na">andExpect</span><span class="o">(</span><span class="n">status</span><span class="o">().</span><span class="na">isOk</span><span class="o">())</span>
                <span class="o">.</span><span class="na">andExpect</span><span class="o">(</span><span class="n">content</span><span class="o">().</span><span class="na">string</span><span class="o">(</span><span class="n">equalTo</span><span class="o">(</span><span class="s">"Hello World"</span><span class="o">)));</span>
    <span class="o">}</span>

<span class="o">}</span>
</code></pre></div></div>

<p><strong>开发环境的调试</strong></p>

<p>热启动在正常开发项目中已经很常见了吧，虽然平时开发web项目过程中，改动项目启重启总是报错；但springBoot对调试支持很好，修改之后可以实时生效，需要添加以下的配置：</p>

<div class="language-xml highlighter-rouge"><div class="highlight"><pre class="highlight"><code> <span class="nt">&lt;dependencies&gt;</span>
    <span class="nt">&lt;dependency&gt;</span>
        <span class="nt">&lt;groupId&gt;</span>org.springframework.boot<span class="nt">&lt;/groupId&gt;</span>
        <span class="nt">&lt;artifactId&gt;</span>spring-boot-devtools<span class="nt">&lt;/artifactId&gt;</span>
        <span class="nt">&lt;optional&gt;</span>true<span class="nt">&lt;/optional&gt;</span>
    <span class="nt">&lt;/dependency&gt;</span>
<span class="nt">&lt;/dependencies&gt;</span>

<span class="nt">&lt;build&gt;</span>
    <span class="nt">&lt;plugins&gt;</span>
        <span class="nt">&lt;plugin&gt;</span>
            <span class="nt">&lt;groupId&gt;</span>org.springframework.boot<span class="nt">&lt;/groupId&gt;</span>
            <span class="nt">&lt;artifactId&gt;</span>spring-boot-maven-plugin<span class="nt">&lt;/artifactId&gt;</span>
            <span class="nt">&lt;configuration&gt;</span>
                <span class="nt">&lt;fork&gt;</span>true<span class="nt">&lt;/fork&gt;</span>
            <span class="nt">&lt;/configuration&gt;</span>
        <span class="nt">&lt;/plugin&gt;</span>
<span class="nt">&lt;/plugins&gt;</span>
<span class="nt">&lt;/build&gt;</span>
</code></pre></div></div>

<p>该模块在完整的打包环境下运行的时候会被禁用。如果你使用java -jar启动应用或者用一个特定的classloader启动，它会认为这是一个“生产环境”。</p>

<h2 id="总结">总结</h2>

<p>使用spring boot可以非常方便、快速搭建项目，使我们不用关心框架之间的兼容性，适用版本等各种问题，我们想使用任何东西，仅仅添加一个配置就可以，所以使用sping boot非常适合构建微服务。</p>

<p><strong><a href="https://github.com/ityouknow/spring-boot-examples">示例代码-github</a></strong></p>

<p><strong><a href="https://gitee.com/ityouknow/spring-boot-examples">示例代码-码云</a></strong></p>

<hr />

<p><strong>作者：纯洁的微笑</strong><br />
<strong>出处：<a href="http://www.ityouknow.com">www.ityouknow.com</a></strong>  <br />
<strong>版权所有，欢迎保留原文链接进行转载：)</strong></p>
转载自：《http://www.ityouknow.com/》纯洁的微笑
