[README.md](https://github.com/user-attachments/files/30050341/README.md)
# 護理實習學生評核系統 — GitHub Pages 部署說明

這個資料夾只有一個檔案 `index.html`，本身就是完整的系統。
它改用 **Firebase Realtime Database** 當共享資料庫（免費），
放到 GitHub Pages 後，就會有一個公開網址，大家連上去看到的是同一份即時資料。

---

## 第一步：建立免費 Firebase 專案（約 5 分鐘）

1. 開啟 https://console.firebase.google.com/ ，用你的 Google 帳號登入
2. 點「新增專案」，輸入名稱（例如 `nd-sn-learning`），一路下一步完成建立
3. 左側選單 → **建構（Build）→ Realtime Database** → 點「建立資料庫」
   - 地區選近的（例如 `asia-southeast1`）
   - 安全性規則先選「**測試模式**」（30 天內開放讀寫，方便先跑起來）
4. 左側齒輪圖示 → **專案設定** → 頁面下方「你的應用程式」→ 點 `</>`（網頁）圖示
   - 應用程式暱稱隨意填，不用勾選 Firebase Hosting
   - 建立後畫面會顯示一段 `firebaseConfig = {...}`，把整個物件複製起來

## 第二步：把設定貼到 index.html

打開 `index.html`，搜尋 `firebaseConfig`，會看到這一段：

```js
const firebaseConfig = {
  apiKey: "請貼上你的 apiKey",
  authDomain: "your-project.firebaseapp.com",
  databaseURL: "https://your-project-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "000000000000",
  appId: "1:000000000000:web:xxxxxxxxxxxxxxxx"
};
```

把整段換成你從 Firebase 複製的那份，存檔。

> 沒設定之前，網頁最上方會出現橘色警告條，資料只留在你自己的瀏覽器；設定完成後警告會消失，資料就會即時共享給所有人。

## 第三步：建立 GitHub Repo 並上傳

1. 到 https://github.com/new 建立一個新 repo（例如 `nd-sn-learning`），設為 **Public**
2. 把這個資料夾裡的 `index.html` 上傳進去（網頁介面拖曳上傳即可，不需要指令列）
3. 進 repo 的 **Settings → Pages**
   - Source 選 `Deploy from a branch`
   - Branch 選 `main` / 資料夾選 `/ (root)` → Save
4. 等 1–2 分鐘，上方會出現網址，格式類似：
   `https://你的帳號.github.io/nd-sn-learning/`

這個網址就是可以分享給老師、學校、學生的正式連結。

---

## 之後要修改內容怎麼辦？

- 每次改 `index.html`（例如加新的技術項目、調整介面），直接在 GitHub 網頁上編輯並 commit，
  GitHub Pages 會自動在 1 分鐘內更新，不需要重新設定 Firebase。

## 資料安全性提醒

「測試模式」規則 30 天後會自動關閉讀寫。到期前建議去 Firebase Console →
Realtime Database → 規則，改成類似下面這種（只要知道資料庫網址加一個簡單密鑰就能用，
比完全公開安全一點，仍算是輕量防護，不是正式帳號權限管控）：

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

如果之後想要更嚴謹的權限（例如老師才能寫入、學校只能讀取），跟我說一聲，
我可以幫你設計搭配 Firebase Authentication 的登入機制。

## 資料匯出

系統內「管理平台 → 資料匯出」可以隨時匯出 CSV / JSON 備份，
建議定期匯出一份存到 Google Drive，作為 Firebase 以外的備援。
