---
title: postMessage
date: 2023-01-11 15:14:34
tags:
---
## [](#vue内嵌iframe并跨域通信 "vue内嵌iframe并跨域通信")vue内嵌iframe并跨域通信

#### [](#近期做新老boss跳转页面-老boss使用sessionStorage存储数据-总结一下遇到的问题 "近期做新老boss跳转页面,老boss使用sessionStorage存储数据,总结一下遇到的问题")近期做新老boss跳转页面,老boss使用sessionStorage存储数据,总结一下遇到的问题

sessionStorage生命周期为当前窗口或标签页，一旦窗口或标签页被永久关闭了，那么所有通过sessionStorage存储的数据也就被清空了。不同浏览器无法共享localStorage或sessionStorage中的信息。相同浏览器的不同页面间可以共享相同的 localStorage（页面属于相同域名和端口），但是不同页面或标签页间无法共享sessionStorage的信息。这里需要注意的是，页面及标 签页仅指顶级窗口，如果一个标签页包含多个iframe标签且他们属于同源页面，那么他们之间是可以共享sessionStorage的。

#### [](#同源的判断规则： "同源的判断规则：")同源的判断规则：

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br></pre></td><td class="code"><pre><span class="line">http://www.test.com</span><br><span class="line">https://www.test.com （不同源，因为协议不同）</span><br><span class="line">http://my.test.com（不同源，因为主机名不同）</span><br><span class="line">http://www.test.com:8080（不同源，因为端口不同）</span><br></pre></td></tr></tbody></table>

#### [](#在此项目原先只用document-cookie-没成功，使用了ifram跨域通信去解决，使用的技术是window-postMessage "在此项目原先只用document.cookie 没成功，使用了ifram跨域通信去解决，使用的技术是window.postMessage")在此项目原先只用document.cookie 没成功，使用了ifram跨域通信去解决，使用的技术是window.postMessage

参考文档： [https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)

> 1、先在vue引入iframe

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br><span class="line">12</span><br><span class="line">13</span><br><span class="line">14</span><br><span class="line">15</span><br><span class="line">16</span><br><span class="line">17</span><br><span class="line">18</span><br><span class="line">19</span><br><span class="line">20</span><br><span class="line">21</span><br><span class="line">22</span><br><span class="line">23</span><br><span class="line">24</span><br></pre></td><td class="code"><pre><span class="line">   &lt;template&gt;</span><br><span class="line">        &lt;div <span class="class"><span class="keyword">class</span></span>=<span class="string">"act-form"</span>&gt;</span><br><span class="line">        &lt;iframe :src=<span class="string">"src"</span>&gt;<span class="xml"><span class="tag">&lt;/<span class="name">iframe</span>&gt;</span></span></span><br><span class="line">        &lt;<span class="regexp">/div&gt;</span></span><br><span class="line"><span class="regexp">    &lt;/</span>template&gt;</span><br><span class="line">    &lt;script&gt;</span><br><span class="line">    <span class="keyword">export</span> <span class="keyword">default</span> {</span><br><span class="line">    data () {</span><br><span class="line">        <span class="keyword">return</span> {</span><br><span class="line">        src: <span class="string">'你的src'</span></span><br><span class="line">        }</span><br><span class="line">    }</span><br><span class="line">    }</span><br><span class="line">    &lt;<span class="regexp">/script&gt;</span></span><br><span class="line"><span class="regexp">```  </span></span><br><span class="line"><span class="regexp"></span></span><br><span class="line"><span class="regexp">&gt;2、操作iframe获取其对象 </span></span><br><span class="line"><span class="regexp">``` javascript</span></span><br><span class="line"><span class="regexp">mounted () {</span></span><br><span class="line"><span class="regexp">    /</span><span class="regexp">/ 这里就拿到了iframe的对象</span></span><br><span class="line"><span class="regexp">    this.$refs.iframe</span></span><br><span class="line"><span class="regexp">    /</span><span class="regexp">/ 这里就拿到了iframe的window对象</span></span><br><span class="line"><span class="regexp">    this.$refs.iframe.contentWindow</span></span><br><span class="line"><span class="regexp">  }</span></span><br></pre></td></tr></tbody></table>

![](/img/iframe1.png)

> 3、vue里面嵌入iframe开始向其传递数据

* * *

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br></pre></td><td class="code"><pre><span class="line">**这里用到了<span class="selector-tag">postMessage</span>   写法 <span class="selector-tag">otherWindow</span><span class="selector-class">.postMessage</span>(<span class="selector-tag">message</span>, <span class="selector-tag">targetOrigin</span>, <span class="selector-attr">[transfer]</span>)**</span><br></pre></td></tr></tbody></table>

\[otherWindow\]

> 其他窗口的一个引用，比如iframe的contentWindow属性、执行window.open返回的窗口对象、或者是命名过或数值索引的window.frames。

\[message\]

> 将要发送到其他 window的数据。它将会被结构化克隆算法序列化。这意味着你可以不受什么限制的将数据对象安全的传送给目标窗口而无需自己序列化

\[targetOrigin\]

> 通过窗口的origin属性来指定哪些窗口能接收到消息事件，其值可以是字符串*（表示无限制）或者一个URI。在发送消息的时候，如果目标窗口的协议、主机地址或端口这三者的任意一项不匹配targetOrigin提供的值，那么消息就不会被发送；只有三者完全匹配，消息才会被发送。这个机制用来控制消息可以发送到哪些窗口；例如，当用postMessage传送密码时，这个参数就显得尤为重要，必须保证它的值与这条包含密码的信息的预期接受者的origin属性完全一致，来防止密码被恶意的第三方截获。如果你明确的知道消息应该发送到哪个窗口，那么请始终提供一个有确切值的targetOrigin，而不是*。不提供确切的目标将导致数据泄露到任何对数据感兴趣的恶意站点。

* * *

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br></pre></td><td class="code"><pre><span class="line">methods: {</span><br><span class="line">    sendMessage (userData) {</span><br><span class="line">        <span class="comment">// 外部vue向iframe内部传数据</span></span><br><span class="line">        <span class="keyword">this</span>.iframeWin.postMessage({</span><br><span class="line">           <span class="comment">// cmd: 'getFormJson',</span></span><br><span class="line">            params: userData</span><br><span class="line">        }, <span class="string">'*'</span>)</span><br><span class="line">    }</span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

这里通过点击事件触发，向iframe发送信息，iframe内部通过如下去处理这条数据

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br></pre></td><td class="code"><pre><span class="line"><span class="comment">// 接受父页面发来的信息</span></span><br><span class="line">       <span class="built_in">window</span>.addEventListener(<span class="string">"message"</span>, <span class="function"><span class="keyword">function</span>(<span class="params">event</span>)</span>{</span><br><span class="line">	<span class="keyword">const</span> data = event.data.params</span><br><span class="line">	sessionStorage.setItem(<span class="string">"user_data"</span>,<span class="built_in">JSON</span>.stringify(data.data));</span><br><span class="line">	sessionStorage.setItem(<span class="string">"listAuth_data"</span>,<span class="built_in">JSON</span>.stringify(data.data.listAuth));</span><br><span class="line">       });</span><br></pre></td></tr></tbody></table>

![](/img/iframe2.png)  
此时已经完全解决这些跨域的参数通信，注意的就是 \* 有指定的域名最好指定 安全为上！