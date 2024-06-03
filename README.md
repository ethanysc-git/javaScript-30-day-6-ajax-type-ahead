# 前端面試

- [完成作品連結](https://java-script-30-day-6-ajax-type-ahead.vercel.app/)

- [連結](https://codepen.io/tariso/pen/LyoaRM) 是 JavaScript 30 Day 6 Ajax Type Ahead 的範例
- **目標** 我希望你能根據上述範例,根據 `Spac` 改寫成你認為最好的解決方案,不限制你用任何的 library,但唯一的限制是希望你能使用任一主流前端框架(Vue、React、Angular)。

## 完成內容

1. 數據整合：
   - 常用選項有二: 1. fetch 2. axios
   - 此次使用 axios 來請求資料，主要的原因為兩者皆為非同步，在此次題目中，差距並不大，但 axios 提供自動數據轉換(response.json())
   - 使用 await/await，並且判定 response.status，搭配 then 以及 catch
2. 搜尋功能：
   - 實作一個搜尋輸入框，以 v-model="searchInput"綁定，搭配@input 及 trimStart()來確保使用者輸入資料的響應及開頭便輸入空白的錯誤
   - 以正規表示法過濾結果
3. 顯示結果：
   - 沿用原網頁的 css，並轉換成 vue 的表示方式
   - 添加了無匹配結果時 template 部分
4. 使用者介面：
   - 沿用原網頁的 css，修改了匹配時 highlight 的部分
   - 可以在不同螢幕大小下正常運作
5. 程式碼組織：
   - 拉出 SearchBox 這一 component，保持每頁及組間之清晰分明
   - 所有命名方式均遵守 clean code 原則
6. 額外挑戰（選擇性）：
   - 實現進階功能 : 增加了 debounce，但考量使用者體驗，僅取 100ms 的延遲，debounce 取自 lodash/function
   - 已修復 bug : 當搜尋框清空時並沒有恢復到最初的狀態
   - 此次部屬到 vercel.app，考慮到 ci/cd 的實作，vercel.app 能夠在 github repository 完成連結的情況下，使 servers 能跟著每個 commit 來自動更新網站內容
