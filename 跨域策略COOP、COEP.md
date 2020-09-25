# 跨域策略COOP、COEP
### 为什么会需要COOP、COEP？
- 因为在调用一些与计算机硬件的接口的时候，可能会遭受Spectre漏洞攻击。
### 什么是Spectre漏洞攻击？
- Spectre 是一个在 CPU 中被发现的漏洞，利用 Spectre ，攻击者可以读取到在统一浏览器下任意 Context Group 下的资源。
   - Context Group是浏览器 Context Group 是一组共享相同上下文的 tab、window或iframe。例如，如果域名a.example网站打开弹出域名为b.example窗口，则打开器窗口和弹出窗口共享相同的浏览上下文，属于同一个Context Group，并且它们可以通过 DOM API相互访问，例如 window.opener。
### 如何阻挡Spectre漏洞攻击？
- 第一步：使用COOP进行隔离。
   - 通过将 COOP 设置为 Cross-Origin-Opener-Policy: same-origin，将把从该网站打开的其他不同源的窗口隔离在不同的浏览器 Context Group，这样就创建的资源的隔离环境。
- 第二步：确认跨域资源被允许加载。
   - 使用CORP(跨源资源策略)明确其被允许加载。CORP有三个值，分别为：
      1. same-site 表示资源只能从同一站点加载。
      ```
      Cross-Origin-Resource-Policy: same-site
      ```
      2. same-origin 表示资源只能从相同的来源加载。 
      ```
      Cross-Origin-Resource-Policy: same-origin
      ```
      3. cross-origin表示资源可以由任何网站加载。
      ```
      Cross-Origin-Resource-Policy: cross-origin
      ```
- 第三步：使用COEP(跨源嵌入程序政策) ,在HTTP Header启用Cross-Origin-Embedder-Policy: require-corp，你可以让你的站点仅加载明确标记为可共享的跨域资源，这样就可以预防Spectre攻击了。

### 参考
- https://web.dev/why-coop-coep/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cross-Origin-Embedder-Policy
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cross-Origin-Opener-Policy
- https://mp.weixin.qq.com/s/vVX__t_LLUQ8Zp2kPOXKpg
