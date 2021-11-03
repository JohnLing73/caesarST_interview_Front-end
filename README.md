# **CaesarST Interview Front-end**
1. 此為畫面產出的流程，有不同的讀取及呈現的時間，一開始如果白頁(FP) 約 2-3 秒後才會產生畫面，請問有什麼方式可以進行確認瓶頸？以及有何方式解決？
> 1. 使用performance analyzer，例如:dev tools 裡的lighthouse 或 pagespeed來找出瓶頸。  
> 2. 解決方式:  
> >1. 可以使用skelton(骨架屏)，先繪出網頁的雛形(但沒圖片、文字等內容，類似 wireframe)，等資料載入完成再繪製到網頁上。  
> > 2.  移除造成阻塞渲染的資源: 可以先使用 lighthouse 偵測出哪些資源是載入後沒有被立即使用的(non-critical)，再用以下的方始處理這些資源。
> > > 1. stylesheet: 把critical 的 stylesheet 從 URL引入改成從行內引入，其餘的 non-critical 使用 preload 屬性下載 
> > > 2. script: 將critical 的 code 移到行內的 script tag，就不須再載入造成阻塞。其餘 non-critical 的 code 視情況加入defer 或 async 屬性。
> > 3.  設定不同 media query 之下的style sheet，如此一來 first paint 時只會受到符合 user 的設備的stylesheet阻塞。
> > 4.  壓縮CSS: 移除多餘空白或字元。

2. 如果為第一頁白頁，到 FP 的時間很久，可能是在那邊產生問題？瀏覽器？後端？網路？請嘗試說明可能性  
> 從白頁到FP的時間可以TTFB(Time To First Byte)指標得知，以下原因均可能造成此 TTFB 過久
> > 1.與網路有關: 
> > 1. user 的網路連線狀態不佳
> > 2. Server 所位在的地區相隔太遠(例如: 從台灣的 Server
> 發送請求到歐洲的 Server)
> 2. 與 Server 有關
> > 1. 後端程式邏輯尚未最佳化，使得抓取 user request 的內容所需時間過長
> > 2. Server 設備效能低下

3. 當頁面進入到 FMP 時，需要進入到最後一個完整畫面，我們會盡量避免畫面跳動，此時可以使用哪些模組？或者哪些技巧讓載入狀況較為順暢，請試著舉例。
> 可使用 [FastDom](https://github.com/wilsonpage/fastdom)，可將 DOM 的 read/ write 各自分離，先行讀取在寫入，避免reflow的狀況發生，讓畫面載入較為順暢。
4. 排版
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    * {
      box-sizing: border-box;
    }
    body {
      margin: 0;
      padding: 0;
    }
    div {
      border: 5px solid black;
      width: 100%;
    }
    header,
    footer {
      padding: 1rem;
    }
    header {
      background-color: rgb(247, 204, 252);
      position: sticky;
      top: 0;
    }
    section {
      display: grid;
      grid-template-columns: 70% 30%;
    }
    aside {
      background-color: rgb(241, 160, 249);
    }
    main {
      min-height: 200vh;
    }
    footer {
      background-color: rgb(193, 140, 238);
    }
    h2 {
      margin: 0;
      font-size: 2rem;
      text-align: center;
    }
    main,
    aside {
      display: flex;
      justify-content: center;
      align-items: center;
    }
  </style>
</head>
<body>
  <div>
    <header>
      <h2>Header</h2>
    </header>
    <section>
      <main>
        <h2>Main Content</h2>
      </main>
      <aside>
        <h2>Right Panel</h2>
      </aside>
    </section>
    <footer>
      <h2>Footer</h2>
    </footer>
  </div>
</body>
</html>
```
5. 改寫function
```js
const sum = add(3)(2);
//sum = 5
console.log(sum);
function add(x) {
  return y => x + y;
}
```
