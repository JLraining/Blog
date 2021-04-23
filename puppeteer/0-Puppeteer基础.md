### 基础
Puppeteer 是一个 Node 库，它提供了高级 API 来通过 DevTools 协议控制 Chromium 或 Chrome。


### 访问网站并截屏
导航到 `https://example.com` 并将截屏保存为 example.png

```
const puppeteer = require('puppeteer');
(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://example.com');
  await page.screenshot({path: 'example.png'});
  await browser.close();
})();
```
上述代码通过puppeteer的launch方法生成了一个browser的实例
browser.newPage方法可以打开一个新选项卡并返回选项卡的实例page，通过page上的各种方法可以对页面进行常用操作。上述代码就进行了截屏的操作。

### page.evaluate(pageFunction, ...args)
一个很强大的方法是page.evaluate(pageFunction, ...args)，可以向页面注入我们的函数
```
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('http://rennaiqian.com');

  // Get the "viewport" of the page, as reported by the page.
  const dimensions = await page.evaluate(() => {
    return {
      width: document.documentElement.clientWidth,
      height: document.documentElement.clientHeight,
      deviceScaleFactor: window.devicePixelRatio
    };
  });

  console.log('Dimensions:', dimensions);
  await browser.close();
})();
```
evaluate方法中是无法直接使用外部的变量的，需要作为参数传入，想要获得执行的结果也需要return出来。

### 文章参考
[Puppeteer实战：教你如何自动在掘金上发布文章-云社区-华为云](https://bbs.huaweicloud.com/blogs/215632)
[自动化 Web 性能分析之 Puppeteer 爬虫实践 - 政采云前端团队](https://www.zoo.team/article/puppeteer)
[awesome-puppeteer](https://github.com/transitive-bullshit/awesome-puppeteer)