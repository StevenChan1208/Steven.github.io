# 0529-0530 筆記整理  

- 標題 :

1. XMLHttpRequest
2. 跨來源資源共用（CORS）
3. .gitignore 
4. npm & npx     
5. axios 用法
6. callback & callback hell
7. Promise
8. Aysnc 與 Await

##  使用XMLHttpRequest

- 透過 XMLHttpRequest 建立的請求，其取得資料的方式可以為非同步（asynchronously）或同步
（synchronously）兩種之一。

- 請求的種類是由 XMLHttpRequest.open() (en-US) 方法的選擇性參數 async（第三個參數）決定。

- 若 async 參數為 true 或是未指定XMLHttpRequest 會被設定為非同步，相反的若為 false 則會被設定為同步。

- 這兩種請求類型的細節討論與示範可以在同步與非同步請求頁面中找到。一般來說，很少會使用到同步請求。

非同步請求 範例

```
asyncBtn.addEventListener("click", function(){
        var xhr = new XMLHttpRequest();
        xhr.open("GET", " http://34.217.120.25:3000/", true);
        xhr.onload = function () {
            message.innerText = `非同步請求 load ${this.responseText}`;
        };
        xhr.send();
    });
       
```
同步請求 範例

```
        syncBtn.addEventListener("click",function(){
            var xhr= new XMLHttpRequest();
            xhr.open("GET","http://34.217.120.25:3000",false);
            xhr.onload= function(response){
                console.log(response);
                message.innerHTML= `同步: ${this.responseText}`; 
            }
            xhr.send();
        });
```

## 跨來源資源共用（CORS）


### 同源政策 (Same-Origin Policy)

首先我們來認識瀏覽器的「同源政策」。

大家應該都有用過瀏覽器提供的 fetch API 或 XMLHttpRequest 等方式，讓我們透過 JavaScript 取得資源。常見的應用是向後端 API 拿取資料再呈現在前端。

需要注意的是，用 JavaScript 透過 fetch API 或 XMLHttpRequest 等方式發起 request，必須遵守同源政策 (same-origin policy)。

什麼是同源政策呢？簡單地說，用 JavaScript 存取資源時，如果是同源的情況下，存取不會受到限制；

然而，在同源政策下，非同源的 request 則會因為安全性的考量受到限制。瀏覽器會強制你遵守 CORS (Cross-Origin Resource Sharing，跨域資源存取) 的規範，否則瀏覽器會讓 request 失敗。

#### 什麼是同源?

那什麼情況是同源，什麼情況不是呢？所謂的同源，必須滿足以下三個條件：

1.相同的通訊協定 (protocol)，即 http/https
2.相同的網域 (domain)
3.相同的通訊埠 (port)

舉例：下列何者與 https://example.com/a.html 為同源？

- <https://example.com/b.html> (true)
- <http://example.com/c.html> (false，不同 protocol)
- <https://subdomain.example.com/d.html> (false，不同 domain)
- <https://example.com:8080/e.html> (false，不同 port)

### 跨來源資源共用（CORS）

跨來源資源共用（Cross-Origin Resource Sharing (CORS)）是一種使用額外 HTTP 標頭令目前瀏覽網站的使用者代理 (en-US)取得存取其他來源（網域）伺服器特定資源權限的機制。當使用者代理請求一個不是目前文件來源——例如來自於不同網域（domain）、通訊協定（protocol）或通訊埠（port）的資源時，會建立一個跨來源 HTTP 請求（cross-origin HTTP request）。
不是同源的情況下，就會產生一個跨來源 http 請求（cross-origin http request）。

舉個例子，例如我想要在 https://shubo.io 的頁面上顯示來自 https://othersite.com 的資料，於是我利用瀏覽器的 fetch API 發送一個請求:

```
try {
  fetch('https://othersite.com/data')
} catch (err) {
  console.error(err);
}
```
這時候就產生了一個跨來源請求。而跨來源請求必須遵守 CORS 的規範。

基於安全性考量，程式碼所發出的跨來源 HTTP 請求會受到限制。例如，XMLHttpRequest 及 Fetch 都遵守同源政策（same-origin policy）。這代表網路應用程式所使用的 API 除非使用 CORS 標頭，否則只能請求與應用程式相同網域的 HTTP 資源。

## .gitignore 

<https://www.toptal.com/developers/gitignore>

.gitignore 要放進 git 裡，讓專案裡的其他人同步

例如:
*.txt 表示忽略所有txt檔
!lib.txt !表示 除了lib.txt
/temp 表示不包括temp文件夾
build/ 忽略build裡的文件
doc/.*txt 忽略 doc/text.txt 但不包括 doc/server/text.txt

## npm

“axios”: “^0.21.1”

主版本.次版本.patch版

主版本: 較大的更新，甚至可不相容前面的版本
次版本: 更新要相容前一個版本
patch版: 修bug,…

^: 只會執行不更改最左邊的非零數字的更新
Allows changes that do not modify the left-most non-zero element in the [major, minor, patch] tuple.

^1.2.3 < 2.0.0
^0.2.3 < 0.3.0

cowsay@1.1.3
^1.1.3
1.5.0

npm install ==> package-lock.json
npm update ==> 更新版本，並且更新 package-lock.json

~: 只更新 patch
~0.13.1
0.13.2

npm install
npm update

npm view cowsay versions (加上s檢視所有版本)
npm view cowsay version 檢視最新版
npm i cowsay@1.1.3 安裝特定版本

加上 .gitignore -> node_modules
npm i cowsay@1.1.3
移除 node_modules
npm i -> 觀察 package-lock 1.1.3
npm update -> 觀察 package-lock 1.5.0
npm i cowsay@1.3.0 -> 觀察 package-lock 1.3.0
package.json, package-lock.json push git
可執行的工具會放在node_modules/.bin
./node_modules/.bin/cowsay
用npx cowsay可以快速使用工具類程式
例如：cowsay

npm i -g cowsay 把牛說話安裝在全域(global)

安裝到全域
npm install -g cowsay

npx cowsay Hi

```
____
< Hi >
-----
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```
### npx

npx:
1.輕鬆地執行本機的命令 （不管是全域的，或是專案底下的）
2.不用安裝命令，就能利用 npx 來執行 （偷偷幫你下載安裝，執行完後刪除）

## callback & callback hell

### Callback



在 JavaScript 中，callback 被當成事件迴圈回頭執行某個已在佇列中進行的程式的目標。再次舉 A、B、C 三件工作的例子，其中 B 必須等待 C 做完才能執行，於是我們將 B 放到 C 的 callback 中，讓宿主環境在收到 C 完成的回應時後 B 放到佇列中準備執行。

```
doA();

doC(function() {
  doB();
});

```
而使用 callback 有兩個主要缺點：(1)「回呼地域」和 (2)「控制權轉移」所造成的信任問題。

### callbacke hell

回呼地域（callback hell）又稱「毀滅金字塔」（pyramid of doom），指層次太深的巢狀 callback，讓程式變得更複雜，難以預測和追蹤。

我們先來看一個簡單的例子，如下，你能一眼看出執行順序嗎？

```
doA(function() {
  doB();

  doC(function() {
    doD();
  });

  doE();
});

doF();

```
答案是？

…

…

要公佈答案摟？

…

…

答案是 doA() -> doF() -> doB() -> doC() -> doE() -> doD()

如果連剛剛上面那個淺淺的巢狀 callback 都難以理解的話，就更不要提層次超深的 callback hell 了 XD

經典的 callback hell 大概是長這個樣子…

callback hell
![](https://i.imgur.com/cuHysRx.png)

Solution: promise

## Promise

在 ES2015 (ES6)中介紹了 Promise — 簡單來說：Callback 以外的另一種方式來處理非同步事件，且可讀性與可維護性比 Callback 好很多。
Promise 是一個物件，代表著一個尚未完成，但最終會完成的一個動作 —在一個「非同步處理」的流程中，它只是一個暫存的值（Placeholder）。

在你點完拿鐵和黑咖啡後，店員給了你一張印有號碼的收據，告訴你等會兒聽到這個號碼，就可以來領咖啡了 — 這張收據就是承諾（Promise），代表這個任務完成後，就可以接著執行接下去的動作了。
等待的過程中，其實你無法百分之百肯定最後一定拿得到咖啡（Pending）：店員可能順利做完咖啡交到你手上（Resolved）；可能牛奶或咖啡豆沒了，所以店員告訴你今天做不出咖啡了（Rejected）

💡 Promise 就像上面的例子，可能處於三種階段中任意階段：
1. Pedning：等待事情完成中，但不確定最終會順利完成或失敗
2. Resolved（或稱 Fulfilled）：代表順利完成了，並轉交結果
3. Rejectesd：代表失敗了，並告知失敗原因

####　建立 Promise 物件實例

一個 Promise 物件是透過 new 關鍵字和它的物件建構子（Constructor）所產生出來的 — 這個建構子函式接收一個帶有 resolve 和 reject 兩個參數的函式（executor）
語法

```
new Promise( function(resolve, reject) { ... } );

```
resolve 函式：當非同步作業成功完成時，將結果作為參數帶入執行
reject 函式：當非同步作業失敗時，將錯誤訊息作為參數帶入執行

then & catch
創建出來的 Promise 物件實例上帶有兩個重要的方法：
- .then() 方法：當成功從 resolve() 獲得結果時 — 狀態由 Pending 轉為 Fulfilled — then() 方法就會被調用。

- catch() 方法：當從 reject() 獲得錯誤訊息時 — 狀態由 Pending 轉為 Rejected — catch() 方法就會被調用來處理錯誤。

#### 串連 then 方法：Promise Chain

還記得那有點誇張的 Callback Hell 😱 嗎？我們依序要向不同伺服器或資料庫獲取資料 — 也就是要「依序」處理多個「非同步」作業 — 所以透過一層層的 Callback 來達成！
從上面 then() 和 catch() 方法的範例可以發現，依據非同步作業執行完成後要處理的後續步驟被獨立了出來，程式碼的可讀性就高很多 — 在依序處理多個「非同步」作業時會更為明顯 — 這是透過 then() 方法的串接：

```

getData
  // 使用 then 方法，並將成功訊息印出來
  .then(data => {
    console.log(data.data1) // abc
    return data.data1 + 'def'
  })
  // 獲得前一個 then() 回傳的結果
  .then(data => {
    console.log(data) // abcdef
    return data + 'ghi'
  })
  // 獲得前一個 then() 回傳的結果
  .then(data => {
    console.log(data) // abcdefghi
  })
  // 使用 catch 方法，並將錯誤訊息印出來
  .catch(error => console.log('錯誤訊息', error))

```

#### Promise.all( […] ) 方法

當在處理「多個非同步事件」時，Promise.all() 方法會等所有 Promise 都被順利完成（Resolved 或稱 Fulfilled）後，才執行接下去的動作；一旦收到某個 Promise 回傳失敗（Rejected），就立即執行後續的錯誤處理流程。


🤔 使用上的好處：縮短等待時間
想像我們要分別向三個資料庫請求資料 — 以下以 setTimeout() 示意類似的非同步處理事件 — 如果能善用 Promise.all() 方法時，我們能先依序向資料庫發出請求，並在成功獲得所有回傳資料後，才執行後續的動作 — 這讓我們不用等到第一個資料庫成功回傳後，才展開第二次請求…成功收到回傳後再展開第三次請求…

```
const oneSecond = new Promise((resolve, reject) => {
  setTimeout(() => {
    // 一秒後回傳資料
    resolve('one second')
  }, 1000);
})

const twoSecond = new Promise((resolve, reject) => {
  setTimeout(() => {
    // 兩秒後回傳資料
    resolve('two second')
  }, 2000);
})

const threeSecond = new Promise((resolve, reject) => {
  setTimeout(() => {
    // 三秒後回傳資料
    resolve('three second')
  }, 3000);
})

// 等到三個 Promise 都成功回傳後，才執行接下去的流程
Promise.all([oneSecond, twoSecond, threeSecond])
  .then(([oneSecond, twoSecond, threeSecond]) => {
    console.log(oneSecond, twoSecond, threeSecond)
  })

  ```

  ## Aysnc / Await
  透過 Promise 包裝和使用，的確避免了 🎄 Callback Hell — Promise Chain 讓整個流程更清楚，提升了程式碼的易讀性與可維護性。

  ### Async / Await 是所謂的語法糖衣

  - async/await 是一種新的語法撰寫方式，來處理「非同步事件」
  - async/await 讓非同步的程式碼讀起來更像在寫「同步程式碼」
  - async/await 是用來簡單化和清楚化 Promise Chain 相對複雜的結構
  - async function 回傳的一樣是 Promise 物件，只是針對 promise-based 寫法進行包裝

  ### Async 關鍵字
   async 關鍵字可以放在任意函式以前，這意味者：「我們正宣告一個非同步的函式，且這個函式會回傳一個 Promise 物件」

    ```
    // 一般函式
    function getGroupInfo() {
    return data
    }

    // 使用 async 函式
    async function getGroupInfo() {
    return  data
    }
    ```
  ### Await 關鍵字

  在 async 函式中使用 await 關鍵字意味者：「我們請 JavaScript 等待這個非同步的作業完成，才展開後續的動作」
  換句話說：await 讓 async 函式的執行動作暫停，等到它獲得回傳的 Promise 物件後--無論執行成功（fulfilled）或失敗（rejected）--才恢復執行 async 函式

  👍優點 1：可以直接將使用 await 後獲得的回傳值存於一個變數中做後續使用，而不是再呼叫 then() 方法一個串一個：這讓程式碼看起來更像平常在處理「同步程式碼」一般，提升了易讀性與可維護性

  👍優點 2：承接優點 1，由於我們將回傳的結果存於一個變數中，這個變數將可以在後續自由的被使用；然而若是透過 then() 方法獲取值來使用，這個值將無法再被帶往下個 then() 方法中使用--要的話也很麻煩

  ### Async / Await範例

  在以下範例中，依然先用 setTimeout() 來模仿類似向資料庫請求和等待資料的非同步事件：

  - 在 getGroupInfo() 函式前加上 async 關鍵字，告知這是一個非同步函式
  - getFirstInfo() 和 getSecondInfo() 是兩個要處理非同步的地方，因此分別在兩個呼叫函式前加上 await 關鍵字

 ```
    function getFirstInfo() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
        resolve('first data')
        }, 1000);
    })
    }

    function getSecondInfo() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
        resolve('second data')
        }, 2000);
    })
    }

    async function getGroupInfo() {
    // 代表等到第一筆資料回傳後，才印出結果和請求第二筆資料
    const firstInfo = await getFirstInfo()
    console.log(firstInfo)
    // 代表等到第二筆資料回傳後，才印出結果
    const secondInfo = await getSecondInfo()
    console.log(secondInfo)
    }

    getGroupInfo()

 ```
  錯誤處理
  當獲得錯誤回傳（Rejected）時，await 會丟出 reject 的值 — 這時可以使用 try…catch…來捕捉錯誤原因：

 ```
  async function getGroupInfo() {
  try {
    const firstInfo = await getFirstInfo()
    console.log(firstInfo)
    const secondInfo = await getSecondInfo()
    console.log(secondInfo)
  } catch (error) {
    // 處理錯誤回傳
    console.log(error)
    }
  }

    getGroupInfo()

  ```  

  ### await 搭配 Promise.all( […] ) 方法

  在以下範例中，依然先用 setTimeout() 來模仿類似向兩個資料庫請求獲取資料的非同步事件：透過 await 關鍵字搭配 Promise.all() 方法，同步向兩個資料庫獲取資料，並等到兩筆資料都成功回傳後，才印出結果 — 省時效率高🤩

 ``` 
    function getFirstInfo() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
        // 1秒後回傳結果
        resolve('first data')
        }, 1000);
    })
    }

    function getSecondInfo() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
        // 2秒後回傳結果
        resolve('second data')
        }, 2000);
    })
    }

    async function getGroupInfo() {
    try {
        const firstInfo = getFirstInfo()
        const secondInfo = getSecondInfo()
        // 同步發出非同步請求，並等到兩秒後（非三秒）成功獲得兩筆回傳後，才印出結果
        const [firstData, secondData] = await Promise.all([firstInfo, secondInfo])
        console.log(firstData, secondData)
    } catch (error) {
        // 處理錯誤回傳
        console.log(error)
    }
    }

    getGroupInfo()
 ``` 

  ## 心得:
     這兩天上課覺得自己還有很多得疑問 就像下面的圖
     
    ![](https://i.imgur.com/4RnEosY.jpg)

     上課的練習 有時候很可怕 拼拼湊湊的程式碼  可怕的試一試之後還跑得動不知道為什麼 蠻臉問號

     最後相信老師說的
      ![](https://i.imgur.com/7tVOofS.jpg)

      今天的我比昨天進步 總有一天能趕上的!