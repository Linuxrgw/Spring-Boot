# Spring-Boot
 <h1>springboot(三)：Spring boot中Redis的使用</h1>
            <span class="meta-info">
                
                
                <span class="octicon octicon-calendar"></span> 2016/03/06
                
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

            <p>spring boot对常用的数据库支持外，对nosql 数据库也进行了封装自动化。</p>

<h2 id="redis介绍">redis介绍</h2>

<p>Redis是目前业界使用最广泛的内存数据存储。相比memcached，Redis支持更丰富的数据结构，例如hashes, lists, sets等，同时支持数据持久化。除此之外，Redis还提供一些类数据库的特性，比如事务，HA，主从库。可以说Redis兼具了缓存系统和数据库的一些特性，因此有着丰富的应用场景。本文介绍Redis在Spring Boot中两个典型的应用场景。</p>

<h2 id="如何使用">如何使用</h2>

<p>1、引入 spring-boot-starter-redis</p>

<div class="language-xml highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nt">&lt;dependency&gt;</span>  
    <span class="nt">&lt;groupId&gt;</span>org.springframework.boot<span class="nt">&lt;/groupId&gt;</span>  
    <span class="nt">&lt;artifactId&gt;</span>spring-boot-starter-redis<span class="nt">&lt;/artifactId&gt;</span>  
<span class="nt">&lt;/dependency&gt;</span>  
</code></pre></div></div>

<p>2、添加配置文件</p>

<div class="language-properties highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="c"># REDIS (RedisProperties)
# Redis数据库索引（默认为0）
</span><span class="py">spring.redis.database</span><span class="p">=</span><span class="s">0  </span>
<span class="c"># Redis服务器地址
</span><span class="py">spring.redis.host</span><span class="p">=</span><span class="s">192.168.0.58</span>
<span class="c"># Redis服务器连接端口
</span><span class="py">spring.redis.port</span><span class="p">=</span><span class="s">6379  </span>
<span class="c"># Redis服务器连接密码（默认为空）
</span><span class="py">spring.redis.password</span><span class="p">=</span>  
<span class="c"># 连接池最大连接数（使用负值表示没有限制）
</span><span class="s">spring.redis.pool.max-active=8  </span>
<span class="c"># 连接池最大阻塞等待时间（使用负值表示没有限制）
</span><span class="py">spring.redis.pool.max-wait</span><span class="p">=</span><span class="s">-1  </span>
<span class="c"># 连接池中的最大空闲连接
</span><span class="py">spring.redis.pool.max-idle</span><span class="p">=</span><span class="s">8  </span>
<span class="c"># 连接池中的最小空闲连接
</span><span class="py">spring.redis.pool.min-idle</span><span class="p">=</span><span class="s">0  </span>
<span class="c"># 连接超时时间（毫秒）
</span><span class="py">spring.redis.timeout</span><span class="p">=</span><span class="s">0  </span>
</code></pre></div></div>

<p>3、添加cache的配置类</p>

<div class="language-java highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nd">@Configuration</span>
<span class="nd">@EnableCaching</span>
<span class="kd">public</span> <span class="kd">class</span> <span class="nc">RedisConfig</span> <span class="kd">extends</span> <span class="n">CachingConfigurerSupport</span><span class="o">{</span>
	
	<span class="nd">@Bean</span>
	<span class="kd">public</span> <span class="n">KeyGenerator</span> <span class="nf">keyGenerator</span><span class="o">()</span> <span class="o">{</span>
        <span class="k">return</span> <span class="k">new</span> <span class="nf">KeyGenerator</span><span class="o">()</span> <span class="o">{</span>
            <span class="nd">@Override</span>
            <span class="kd">public</span> <span class="n">Object</span> <span class="nf">generate</span><span class="o">(</span><span class="n">Object</span> <span class="n">target</span><span class="o">,</span> <span class="n">Method</span> <span class="n">method</span><span class="o">,</span> <span class="n">Object</span><span class="o">...</span> <span class="n">params</span><span class="o">)</span> <span class="o">{</span>
                <span class="n">StringBuilder</span> <span class="n">sb</span> <span class="o">=</span> <span class="k">new</span> <span class="n">StringBuilder</span><span class="o">();</span>
                <span class="n">sb</span><span class="o">.</span><span class="na">append</span><span class="o">(</span><span class="n">target</span><span class="o">.</span><span class="na">getClass</span><span class="o">().</span><span class="na">getName</span><span class="o">());</span>
                <span class="n">sb</span><span class="o">.</span><span class="na">append</span><span class="o">(</span><span class="n">method</span><span class="o">.</span><span class="na">getName</span><span class="o">());</span>
                <span class="k">for</span> <span class="o">(</span><span class="n">Object</span> <span class="n">obj</span> <span class="o">:</span> <span class="n">params</span><span class="o">)</span> <span class="o">{</span>
                    <span class="n">sb</span><span class="o">.</span><span class="na">append</span><span class="o">(</span><span class="n">obj</span><span class="o">.</span><span class="na">toString</span><span class="o">());</span>
                <span class="o">}</span>
                <span class="k">return</span> <span class="n">sb</span><span class="o">.</span><span class="na">toString</span><span class="o">();</span>
            <span class="o">}</span>
        <span class="o">};</span>
    <span class="o">}</span>

    <span class="nd">@SuppressWarnings</span><span class="o">(</span><span class="s">"rawtypes"</span><span class="o">)</span>
    <span class="nd">@Bean</span>
    <span class="kd">public</span> <span class="n">CacheManager</span> <span class="nf">cacheManager</span><span class="o">(</span><span class="n">RedisTemplate</span> <span class="n">redisTemplate</span><span class="o">)</span> <span class="o">{</span>
        <span class="n">RedisCacheManager</span> <span class="n">rcm</span> <span class="o">=</span> <span class="k">new</span> <span class="n">RedisCacheManager</span><span class="o">(</span><span class="n">redisTemplate</span><span class="o">);</span>
        <span class="c1">//设置缓存过期时间</span>
        <span class="c1">//rcm.setDefaultExpiration(60);//秒</span>
        <span class="k">return</span> <span class="n">rcm</span><span class="o">;</span>
    <span class="o">}</span>
    
    <span class="nd">@Bean</span>
    <span class="kd">public</span> <span class="n">RedisTemplate</span><span class="o">&lt;</span><span class="n">String</span><span class="o">,</span> <span class="n">String</span><span class="o">&gt;</span> <span class="nf">redisTemplate</span><span class="o">(</span><span class="n">RedisConnectionFactory</span> <span class="n">factory</span><span class="o">)</span> <span class="o">{</span>
        <span class="n">StringRedisTemplate</span> <span class="n">template</span> <span class="o">=</span> <span class="k">new</span> <span class="n">StringRedisTemplate</span><span class="o">(</span><span class="n">factory</span><span class="o">);</span>
        <span class="n">Jackson2JsonRedisSerializer</span> <span class="n">jackson2JsonRedisSerializer</span> <span class="o">=</span> <span class="k">new</span> <span class="n">Jackson2JsonRedisSerializer</span><span class="o">(</span><span class="n">Object</span><span class="o">.</span><span class="na">class</span><span class="o">);</span>
        <span class="n">ObjectMapper</span> <span class="n">om</span> <span class="o">=</span> <span class="k">new</span> <span class="n">ObjectMapper</span><span class="o">();</span>
        <span class="n">om</span><span class="o">.</span><span class="na">setVisibility</span><span class="o">(</span><span class="n">PropertyAccessor</span><span class="o">.</span><span class="na">ALL</span><span class="o">,</span> <span class="n">JsonAutoDetect</span><span class="o">.</span><span class="na">Visibility</span><span class="o">.</span><span class="na">ANY</span><span class="o">);</span>
        <span class="n">om</span><span class="o">.</span><span class="na">enableDefaultTyping</span><span class="o">(</span><span class="n">ObjectMapper</span><span class="o">.</span><span class="na">DefaultTyping</span><span class="o">.</span><span class="na">NON_FINAL</span><span class="o">);</span>
        <span class="n">jackson2JsonRedisSerializer</span><span class="o">.</span><span class="na">setObjectMapper</span><span class="o">(</span><span class="n">om</span><span class="o">);</span>
        <span class="n">template</span><span class="o">.</span><span class="na">setValueSerializer</span><span class="o">(</span><span class="n">jackson2JsonRedisSerializer</span><span class="o">);</span>
        <span class="n">template</span><span class="o">.</span><span class="na">afterPropertiesSet</span><span class="o">();</span>
        <span class="k">return</span> <span class="n">template</span><span class="o">;</span>
    <span class="o">}</span>

<span class="o">}</span>

</code></pre></div></div>

<p>3、好了，接下来就可以直接使用了</p>

<div class="language-java highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nd">@RunWith</span><span class="o">(</span><span class="n">SpringJUnit4ClassRunner</span><span class="o">.</span><span class="na">class</span><span class="o">)</span>
<span class="nd">@SpringApplicationConfiguration</span><span class="o">(</span><span class="n">Application</span><span class="o">.</span><span class="na">class</span><span class="o">)</span>
<span class="kd">public</span> <span class="kd">class</span> <span class="nc">TestRedis</span> <span class="o">{</span>

    <span class="nd">@Autowired</span>
    <span class="kd">private</span> <span class="n">StringRedisTemplate</span> <span class="n">stringRedisTemplate</span><span class="o">;</span>
    
	<span class="nd">@Autowired</span>
	<span class="kd">private</span> <span class="n">RedisTemplate</span> <span class="n">redisTemplate</span><span class="o">;</span>

    <span class="nd">@Test</span>
    <span class="kd">public</span> <span class="kt">void</span> <span class="nf">test</span><span class="o">()</span> <span class="kd">throws</span> <span class="n">Exception</span> <span class="o">{</span>
        <span class="n">stringRedisTemplate</span><span class="o">.</span><span class="na">opsForValue</span><span class="o">().</span><span class="na">set</span><span class="o">(</span><span class="s">"aaa"</span><span class="o">,</span> <span class="s">"111"</span><span class="o">);</span>
        <span class="n">Assert</span><span class="o">.</span><span class="na">assertEquals</span><span class="o">(</span><span class="s">"111"</span><span class="o">,</span> <span class="n">stringRedisTemplate</span><span class="o">.</span><span class="na">opsForValue</span><span class="o">().</span><span class="na">get</span><span class="o">(</span><span class="s">"aaa"</span><span class="o">));</span>
    <span class="o">}</span>
    
    <span class="nd">@Test</span>
    <span class="kd">public</span> <span class="kt">void</span> <span class="nf">testObj</span><span class="o">()</span> <span class="kd">throws</span> <span class="n">Exception</span> <span class="o">{</span>
        <span class="n">User</span> <span class="n">user</span><span class="o">=</span><span class="k">new</span> <span class="n">User</span><span class="o">(</span><span class="s">"aa@126.com"</span><span class="o">,</span> <span class="s">"aa"</span><span class="o">,</span> <span class="s">"aa123456"</span><span class="o">,</span> <span class="s">"aa"</span><span class="o">,</span><span class="s">"123"</span><span class="o">);</span>
        <span class="n">ValueOperations</span><span class="o">&lt;</span><span class="n">String</span><span class="o">,</span> <span class="n">User</span><span class="o">&gt;</span> <span class="n">operations</span><span class="o">=</span><span class="n">redisTemplate</span><span class="o">.</span><span class="na">opsForValue</span><span class="o">();</span>
        <span class="n">operations</span><span class="o">.</span><span class="na">set</span><span class="o">(</span><span class="s">"com.neox"</span><span class="o">,</span> <span class="n">user</span><span class="o">);</span>
        <span class="n">operations</span><span class="o">.</span><span class="na">set</span><span class="o">(</span><span class="s">"com.neo.f"</span><span class="o">,</span> <span class="n">user</span><span class="o">,</span><span class="mi">1</span><span class="o">,</span><span class="n">TimeUnit</span><span class="o">.</span><span class="na">SECONDS</span><span class="o">);</span>
        <span class="n">Thread</span><span class="o">.</span><span class="na">sleep</span><span class="o">(</span><span class="mi">1000</span><span class="o">);</span>
        <span class="c1">//redisTemplate.delete("com.neo.f");</span>
        <span class="kt">boolean</span> <span class="n">exists</span><span class="o">=</span><span class="n">redisTemplate</span><span class="o">.</span><span class="na">hasKey</span><span class="o">(</span><span class="s">"com.neo.f"</span><span class="o">);</span>
        <span class="k">if</span><span class="o">(</span><span class="n">exists</span><span class="o">){</span>
        	<span class="n">System</span><span class="o">.</span><span class="na">out</span><span class="o">.</span><span class="na">println</span><span class="o">(</span><span class="s">"exists is true"</span><span class="o">);</span>
        <span class="o">}</span><span class="k">else</span><span class="o">{</span>
        	<span class="n">System</span><span class="o">.</span><span class="na">out</span><span class="o">.</span><span class="na">println</span><span class="o">(</span><span class="s">"exists is false"</span><span class="o">);</span>
        <span class="o">}</span>
       <span class="c1">// Assert.assertEquals("aa", operations.get("com.neo.f").getUserName());</span>
    <span class="o">}</span>
<span class="o">}</span>

</code></pre></div></div>

<p>以上都是手动使用的方式，如何在查找数据库的时候自动使用缓存呢，看下面；</p>

<p>4、自动根据方法生成缓存</p>

<div class="language-java highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nd">@RequestMapping</span><span class="o">(</span><span class="s">"/getUser"</span><span class="o">)</span>
<span class="nd">@Cacheable</span><span class="o">(</span><span class="n">value</span><span class="o">=</span><span class="s">"user-key"</span><span class="o">)</span>
<span class="kd">public</span> <span class="n">User</span> <span class="nf">getUser</span><span class="o">()</span> <span class="o">{</span>
    <span class="n">User</span> <span class="n">user</span><span class="o">=</span><span class="n">userRepository</span><span class="o">.</span><span class="na">findByUserName</span><span class="o">(</span><span class="s">"aa"</span><span class="o">);</span>
    <span class="n">System</span><span class="o">.</span><span class="na">out</span><span class="o">.</span><span class="na">println</span><span class="o">(</span><span class="s">"若下面没出现“无缓存的时候调用”字样且能打印出数据表示测试成功"</span><span class="o">);</span>  
    <span class="k">return</span> <span class="n">user</span><span class="o">;</span>
<span class="o">}</span>
</code></pre></div></div>
<p>其中value的值就是缓存到redis中的key</p>

<h2 id="共享session-spring-session-data-redis">共享Session-spring-session-data-redis</h2>

<p>分布式系统中，sessiong共享有很多的解决方案，其中托管到缓存中应该是最常用的方案之一，</p>

<h3 id="spring-session官方说明">Spring Session官方说明</h3>

<p>Spring Session provides an API and implementations for managing a user’s session information.</p>

<h3 id="如何使用-1">如何使用</h3>

<p>1、引入依赖</p>

<div class="language-xml highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nt">&lt;dependency&gt;</span>
    <span class="nt">&lt;groupId&gt;</span>org.springframework.session<span class="nt">&lt;/groupId&gt;</span>
    <span class="nt">&lt;artifactId&gt;</span>spring-session-data-redis<span class="nt">&lt;/artifactId&gt;</span>
<span class="nt">&lt;/dependency&gt;</span>
</code></pre></div></div>

<p>2、Session配置：</p>

<div class="language-java highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nd">@Configuration</span>
<span class="nd">@EnableRedisHttpSession</span><span class="o">(</span><span class="n">maxInactiveIntervalInSeconds</span> <span class="o">=</span> <span class="mi">86400</span><span class="o">*</span><span class="mi">30</span><span class="o">)</span>
<span class="kd">public</span> <span class="kd">class</span> <span class="nc">SessionConfig</span> <span class="o">{</span>
<span class="o">}</span>
</code></pre></div></div>

<blockquote>
  <p>maxInactiveIntervalInSeconds: 设置Session失效时间，使用Redis Session之后，原Boot的server.session.timeout属性不再生效</p>
</blockquote>

<p>好了，这样就配置好了，我们来测试一下</p>

<p>3、测试</p>

<p>添加测试方法获取sessionid</p>

<div class="language-java highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nd">@RequestMapping</span><span class="o">(</span><span class="s">"/uid"</span><span class="o">)</span>
    <span class="n">String</span> <span class="nf">uid</span><span class="o">(</span><span class="n">HttpSession</span> <span class="n">session</span><span class="o">)</span> <span class="o">{</span>
        <span class="n">UUID</span> <span class="n">uid</span> <span class="o">=</span> <span class="o">(</span><span class="n">UUID</span><span class="o">)</span> <span class="n">session</span><span class="o">.</span><span class="na">getAttribute</span><span class="o">(</span><span class="s">"uid"</span><span class="o">);</span>
        <span class="k">if</span> <span class="o">(</span><span class="n">uid</span> <span class="o">==</span> <span class="kc">null</span><span class="o">)</span> <span class="o">{</span>
            <span class="n">uid</span> <span class="o">=</span> <span class="n">UUID</span><span class="o">.</span><span class="na">randomUUID</span><span class="o">();</span>
        <span class="o">}</span>
        <span class="n">session</span><span class="o">.</span><span class="na">setAttribute</span><span class="o">(</span><span class="s">"uid"</span><span class="o">,</span> <span class="n">uid</span><span class="o">);</span>
        <span class="k">return</span> <span class="n">session</span><span class="o">.</span><span class="na">getId</span><span class="o">();</span>
    <span class="o">}</span>
</code></pre></div></div>

<p>登录redis 输入 <code class="highlighter-rouge">keys '*sessions*'</code></p>
<div class="language-xml highlighter-rouge"><div class="highlight"><pre class="highlight"><code>t<span class="nt">&lt;spring:session:sessions:db031986-8ecc-48d6-b471-b137a3ed6bc4</span>
<span class="err">t(spring:session:expirations:1472976480000</span>
</code></pre></div></div>
<p>其中 1472976480000为失效时间，意思是这个时间后session失效，<code class="highlighter-rouge">db031986-8ecc-48d6-b471-b137a3ed6bc4</code> 为sessionId,登录http://localhost:8080/uid 发现会一致，就说明session 已经在redis里面进行有效的管理了。</p>

<h3 id="如何在两台或者多台中共享session">如何在两台或者多台中共享session</h3>

<p>其实就是按照上面的步骤在另一个项目中再次配置一次，启动后自动就进行了session共享。</p>

<p><strong><a href="https://github.com/ityouknow/spring-boot-examples">示例代码-github</a></strong></p>

<p><strong><a href="https://gitee.com/ityouknow/spring-boot-examples">示例代码-码云</a></strong></p>

<h2 id="参考">参考</h2>

<p><a href="http://emacoo.cn/blog/spring-redis">Redis的两个典型应用场景</a><br />
<a href="https://segmentfault.com/a/1190000004358410">SpringBoot应用之分布式会话</a></p>

<hr />

<p><strong></strong><br />
<strong>出处：<a href="http://www.ityouknow.com">www.ityouknow.com</a></strong>  转载自：纯洁的微笑  <br />
<strong>版权所有，欢迎保留原文链接进行转载：)</strong></p>
