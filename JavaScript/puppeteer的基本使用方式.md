# 参考资料

# 2. 示例代码

```javascript
// Import puppeteer
import puppeteer from 'puppeteer';

(async () => {
    // Launch the browser
    const browser = await puppeteer.launch({
        headless: false,
        executablePath: "C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe",
        userDataDir: './my-chrome-user-data',
        ignoreDefaultArgs: ['--disable-extensions']
    });
    
    // Create a page
    const page = await browser.newPage();
    
    // Go to your site
    await page.goto('https://pan.baidu.com/s/1CJx0lEPP4kV2A0wXX0XK9Q?pwd=m4wi');
    await page.setViewport({width: 1440, height: 1080});
    
    // page.
    // Query for an element handle.
    // const inputElement = await page.waitForSelector('input.s_ipt');
    // await inputElement.type('oppo 官网');
    //
    // const searchButton = await page.waitForSelector('#submitBtn');
    //
    // await searchButton.click();
    // Do something with element...
    // await element.click(); // Just an example.
    
    // Dispose of handle
    // await elementnt.dispose();
    
    // Close browser.
    // await browser.close();
})();

for (let item of document.querySelectorAll('.treeview-txt'))  {
    const text = item.innerHTML;
    
    if (text === 'QW444') {
        // console.log('item.parentElement.parentElement', item.parentElement.parentElement)
        
        // 选中要保存的文件夹
        item.parentElement.parentElement.click();
        break;
    }
}


// 保存按钮
document.querySelector('.g-button.g-button-blue-large').click();
```