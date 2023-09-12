[奶奶都能轻松入门的 Puppeteer 教程](https://juejin.cn/post/7047462936293408776)

page.on是Puppeteer库中Page对象的方法之一，用于注册和监听不同类型的事件。通过page.on方法，您可以为Page对象注册以下类型的事件处理程序：

'console': 当页面中的console对象触发事件时，比如调用console.log、console.error等方法时。  
'dialog': 当页面弹出对话框时，比如alert、confirm、prompt等。  
'error': 当页面中发生JavaScript错误时。  
'pageerror': 当页面中发生未捕获的JavaScript错误时。  
'request': 当页面发起网络请求时。  
'response': 当页面接收到网络响应时。  
'requestfailed': 当页面中的请求失败时。  
'requestfinished': 当页面中的请求完成时。  
'domcontentloaded': 当页面的DOMContentLoaded事件触发时。  
'load': 当页面的load事件触发时。  
'frameattached': 当页面中的iframe被附加时。  
'framedetached': 当页面中的iframe被移除时。  
'framenavigated': 当页面中的iframe导航到新的URL时。  
'load': 当页面的load事件触发时。  
'close': 当页面关闭时。  
以下是一个示例，演示如何使用page.on方法监听console事件：

```less
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();

  page.on('console', (message) => {
    console.log(`Console message: ${message.text()}`);
  });

  await page.goto('https://example.com');
  await page.evaluate(() => {
    console.log('Hello, world!');
  });

  await browser.close();
})();
```

您可以根据需要注册和监听其他类型的事件，使用相应的事件类型字符串作为page.on的第一个参数，然后提供相应的事件处理程序函数。

在page.on('request', handler)​​中，handler​​是一个回调函数，用于处理页面发起的请求。当页面发起请求时，handler​​将被调用，并接收一个Request​​对象作为参数。

在Request​​对象中，有一个resourceType​​属性，表示请求的资源类型。resourceType​​的值是一个字符串，用于标识请求的资源类型。以下是一些常见的resourceType​​类型：

1. ​'document'​​: HTML文档。
2. ​'stylesheet'​​: 样式表。
3. ​'image'​​: 图像文件。
4. ​'media'​​: 媒体文件，如音频或视频。
5. ​'font'​​: 字体文件。
6. ​'script'​​: JavaScript脚本文件。
7. ​'xhr'​​: XMLHttpRequest请求。
8. ​'fetch'​​: Fetch请求。
9. ​'websocket'​​: WebSocket连接。
10. ​'manifest'​​: 网页清单文件。
11. ​'other'​​: 其他类型的资源。

您可以通过检查Request​​对象的resourceType​​属性来确定请求的资源类型，并根据需要执行相应的操作。以下是一个示例，演示如何使用page.on​​方法监听特定资源类型的请求：

```less
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();

  page.on('request', (request) => {
    if (request.resourceType() === 'image') {
      console.log(`Image request URL: ${request.url()}`);
    }
  });

  await page.goto('https://example.com');

  await browser.close();
})();
```

在上面的示例中，我们注册了一个request​​事件处理程序，当页面发起请求时，如果请求的资源类型是'image'​​，则将请求的URL打印到控制台上。

通过检查Request​​对象的resourceType​​属性，您可以根据需要处理特定类型的请求。

XHR请求与Fetch请求的区别：

XHR（XMLHttpRequest）和Fetch都是用于在Web浏览器中发起HTTP请求的技术，但它们之间有一些重要的区别。以下是XHR请求和Fetch请求之间的主要区别：

1. API 设计和用法：XHR是在早期的Web开发中引入的，它的API相对较为复杂。它使用回调函数、事件监听器和状态码来处理请求和响应。相比之下，Fetch是ES6中引入的新API，它使用了Promise和Response对象，提供了更简洁、现代化的API设计。
2. 返回值：XHR的responseText​​属性包含响应的文本内容，responseXML​​属性包含响应的XML文档。而Fetch通过返回一个Promise来处理响应，并将响应解析为一个Response对象。您可以使用Response对象上的方法（如text()​​、json()​​、blob()​​等）来获取响应的内容。
3. 默认行为：XHR请求会自动发送当前页面的Cookies和HTTP身份验证信息，而Fetch请求默认不会发送Cookies和身份验证信息，需要手动配置。
4. 跨域请求：XHR请求受到同源策略的限制，需要配置CORS（跨域资源共享）以进行跨域请求。Fetch请求在默认情况下支持跨域请求，并且可以使用CORS头部来进一步控制跨域请求的行为。
5. 取消请求：XHR可以使用abort()​​方法取消正在进行的请求，而Fetch目前没有直接提供取消请求的方法。但可以使用控制器（AbortController）和信号（AbortSignal）来实现类似的功能。

总的来说，Fetch提供了更简洁、现代化的API设计，并且在处理异步操作和响应处理方面更加灵活。然而，XHR仍然在一些旧的Web应用程序和库中广泛使用，并且具有更广泛的浏览器支持。选择使用XHR还是Fetch取决于您的特定需求和目标浏览器环境。

在 Puppeteer 中，Protocol.Network.ResourceTiming​​ 是一个表示资源时间信息的对象。它包含了与网络请求相关的时间数据，例如发起请求的时间、接收到第一个字节的时间、完成请求的时间等。

​Protocol.Network.ResourceTiming​​ 对象的属性如下：

- ​startTime​​：资源请求开始的时间戳。
- ​domainLookupStart​​：域名解析开始的时间戳。
- ​domainLookupEnd​​：域名解析结束的时间戳。
- ​connectStart​​：建立连接开始的时间戳。
- ​connectEnd​​：建立连接结束的时间戳。
- ​secureConnectionStart​​：安全连接开始的时间戳。
- ​requestStart​​：发送请求开始的时间戳。
- ​responseStart​​：接收到第一个字节的时间戳。
- ​responseEnd​​：接收完整响应的时间戳。

这些时间戳是相对于页面导航开始的时间来计算的，以毫秒为单位。通过分析这些时间数据，您可以了解资源请求的各个阶段所花费的时间，以及网络性能方面的指标。

要获取资源的时间信息，可以通过 HTTPRequest​​ 对象的 timing()​​ 方法来获得。例如：

```
const request = /* 获取到的 HTTPRequest 对象 */;
const timing = await request.timing();
console.log(timing);
```

​timing​​ 对象将包含 Protocol.Network.ResourceTiming​​ 的属性。

请注意，获取资源时间信息需要启用 Network Tracing（网络跟踪）功能，以及确保在请求完成后获取时间信息。详细的启用网络跟踪的方法，请参考之前提供的关于启用 Network Tracing 的回答。