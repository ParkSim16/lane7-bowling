# Lane 7 保齡球：給 Claude 的工作規則

整個遊戲在 `bowling.html` 一個檔案裡（three.js + cannon-es，從 CDN 載入）。用 `.claude/launch.json` 的 `bowling` 設定預覽。
跟使用者溝通用繁體中文。

## 修改與推送流程

1. 改動先留在本機，讓使用者試玩、確認。**使用者說可以之後才 commit 和推送。**
2. 每次推上 GitHub 都要發一個新版號（語意化版本 `主版.次版.修正`）：
   - 修 bug、微調數值 → 修正號 +1（1.1.0 → 1.1.1）
   - 新功能 → 次版號 +1，修正號歸零（1.1.1 → 1.2.0）
   - 玩法或存檔不相容的大改 → 主版號 +1
3. 推送前要一起更新：
   - `bowling.html` 裡的 `GAME_VERSION`
   - `CHANGELOG.md`：最上面加一段 `## [版號] - 日期`，用「新增／變更／修正／移除」分類，寫玩家看得懂的說明
4. commit 訊息開頭寫版號，例如 `v1.2.0: 加入多人輪流`。
5. 加上附註標籤並推送：
   ```bash
   git tag -a v1.2.0 -m "v1.2.0"
   git push origin main --follow-tags
   ```
6. 建立 GitHub Release，內容就是 CHANGELOG 裡那一版的段落：
   ```bash
   gh release create v1.2.0 --title "v1.2.0" --notes "..."
   ```
7. 不要改寫已推送的歷史（不 force push、不刪 tag）。要退版就把舊版檔案拿回來，發成新的修正版（做法見 README「退回舊版本」）。
