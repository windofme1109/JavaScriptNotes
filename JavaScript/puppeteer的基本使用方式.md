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
    // const extractFileButton = await page.waitForSelector('.submit-btn-text');
    //
    // await extractFileButton.click();
    
    const saveFileToDiskButton = await page.waitForSelector('.g-button.tools-share-save-hb');
    await saveFileToDiskButton.click();
    
    console.log(1111)
    // await page.waitForSelector('.dialog-body');
    // await page.waitForNavigation()
    await page.waitForSelector('.dialog-body .treeview.treeview-root-content.treeview-content li.treeview- .treeview-txt');
    
    
    const targetFolderListElementList = await page.$$('.dialog-body .treeview.treeview-root-content.treeview-content li.treeview-');
    
    for (let ele of targetFolderListElementList) {
        
        const folderEle = await ele.$('span.treeview-txt');
        
        const folderName = await page.evaluate(el => el.innerHTML, folderEle);
        
        console.log('1', folderName);
        
        if (folderName === '60fps-Animation') {
            await ele.click();
            
            await ele.waitForSelector('ul.treeview.treeview-content li');
            
            const list = await ele.$$('ul.treeview.treeview-content li.treeview-');
            // console.log(list)
            for (let s of list) {
                const folderEle = await s.$('span.treeview-txt');
                
                const folderName = await page.evaluate(el => el.innerHTML, folderEle);
                
                console.log('2', folderName);
                
                if (folderName === '2023') {
                    await s.click();
                    await s.waitForSelector('ul.treeview.treeview-content li');
                    
                    const list = await s.$$('ul.treeview.treeview-content li.treeview-');
                    // console.log(list)
                    for (let s of list) {
                        const folderEle = await s.$('span.treeview-txt');
                        
                        const folderName = await page.evaluate(el => el.innerHTML, folderEle);
                        
                        console.log('3', folderName);
                        
                        if (folderName === 'QW444') {
                            await s.click()
                        }
                    
                    }
                }
            
            }
        
        }
    }
    
    // targetFolderListElement.forEach(item => {
    //     item.click()
    // })
    
    // console.log(targetFolderListElement.length)
    // console.log(targetFolderListElement)
    // for (let  t of targetFolderListElement) {
    //     console.log(t.toString())
    // }
    
    // const options = await page.$$eval('.dialog-body .treeview.treeview-root-content.treeview-content li.treeview- .treeview-txt', (el) => {
    //     el.forEach(item => {
    //
    //         const folderName = item
    //
    //         console.log('item.innerHTML', item.innerHTML)
    //     })
    //     return el.map(item => item.innerHTML)
    // });
    //
    // console.log('options', options)
    
    
    // const confirmSaveFileButton = await page.waitForSelector('.g-button.g-button-blue-large');
    
    
    
    
    
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

// for (let item of document.querySelectorAll('.treeview-txt'))  {
//     const text = item.innerHTML;
//
//     if (text === 'QW444') {
//         // console.log('item.parentElement.parentElement', item.parentElement.parentElement)
//
//         // 选中要保存的文件夹
//         item.parentElement.parentElement.click();
//         break;
//     }
// }


// 保存按钮
// document.querySelector('.g-button.g-button-blue-large').click();
```