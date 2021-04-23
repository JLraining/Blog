### 访问网站并截屏
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
### 有界面
关掉无界面模式，有时查看浏览器显示的内容是很有用的。使用以下命令可以启动完整版浏览器
```
const browser = await puppeteer.launch({headless: false})
```
### 捕捉页面信息 document实例
const el = await page.$(selector)

page.evaluate(pageFunction, ...args)
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
### 减慢速度
slowMo选项以指定的毫秒减慢Puppeteer的操作
```
const browser = await puppeteer.launch({
  headless:false,
  slowMo:250
});
```
### 捕获console的输出,通过监听console事件
```
page.on('console', msg => console.log('PAGE LOG:', ...msg.args));
await page.evaluate(() => console.log(`url is ${location.href}`));
```

### 模拟设备
```
const puppeteer = require('puppeteer');
const devices = require('puppeteer/DeviceDescriptors');
const iPhone = devices['iPhone 6'];
puppeteer.launch().then(async browser => {
  const page = await browser.newPage();
  await page.emulate(iPhone);
  await page.goto('https://www.example.com');
  // other actions...
  await browser.close();
});
```

### 自动输入
* 点击使用page.click(selector[, options])方法，
* 聚焦page.focus(selector)。
* 输入使用page.type(selector, text[, options])输入指定的字符串，还可以在options中设置delay缓慢输入更像真人一些。
* 使用keyboard.down(key[, options])来一个字符一个字符的输入
* 模拟回车键 page.keyboard.press('Enter');