# Lane 7 保齡球

第一人稱 3D 保齡球遊戲，單一 HTML 檔，使用 three.js 繪圖、cannon-es 做物理模擬（皆由 CDN 載入）。

## 玩法

- **丟球**：按住畫面往上滑，放開出手；滑得越快球速越快
- **瞄準**：斜著滑，球道上會出現瞄準線
- **曲球**：滑出弧線，球會在球道後段轉彎
- **站位**：← → 鍵或畫面兩側按鈕，一次移動一塊板
- **暫停**：左上角暫停鈕、Esc 或 P

標準十格計分（第十格最多三球），最佳分數存在瀏覽器 localStorage。

## 版本

目前版本寫在 `bowling.html` 的 `GAME_VERSION`，遊戲開始畫面和暫停選單都看得到。
每一版改了什麼請看 [CHANGELOG.md](CHANGELOG.md)，GitHub 的 Releases 頁面也有同樣的說明。

### 查看所有版本

```bash
git tag -l
```

### 退回舊版本

只想**看看或試玩**舊版（不影響目前的程式）：

```bash
git checkout v1.0.0
```

看完回到最新版：

```bash
git checkout main
```

確定要**把正式版本退回**舊版：不要刪掉後來的紀錄，而是把舊版的檔案拿回來，再當成新的一版推上去（例如 v1.1.0 有問題，退回 v1.0.0 的內容並發布成 v1.1.1）：

```bash
git checkout v1.0.0 -- bowling.html
```

然後照一般流程更新版號、CHANGELOG，commit、加 tag、推上去。這樣每一步都留有紀錄，之後還能再回到任何一版。

## 本機執行

用任何靜態伺服器開啟 `bowling.html`，或用內附的小伺服器：

```bash
node -e "const h=require('http'),f=require('fs');h.createServer((q,s)=>{f.readFile('bowling.html',(e,d)=>{s.writeHead(200,{'Content-Type':'text/html; charset=utf-8'});s.end('<!doctype html><html><head><meta charset=utf-8><meta name=viewport content=\"width=device-width,initial-scale=1,viewport-fit=cover\"></head><body>'+d+'</body></html>')})}).listen(5317)"
```

然後打開 http://localhost:5317 。（`.claude/launch.json` 也有同一個設定，給 Claude Code 預覽用。）

## 程式結構（bowling.html）

- 球道尺寸常數：`W`、`R`、`HEAD_Z` 等（公尺，依真實 60 呎球道）
- 物理調校：`cm(...)` 接觸材質（摩擦、彈性）、`preStep` 中的曲球側向力
- 滑動判定：`analyze()` 把滑動軌跡換算成球速、角度、旋轉
- 計分：`marksFor()`、`totals()`、`evaluate()`
- 多人輪流：`buildPlayers()`、`usePlayer()`、`afterResult()`（換人／換格）、`gameOver()`（排行）
- 音效（Web Audio，除了撞瓶錄音都是即時合成）：`initAudio()`、`setRolling()`（滾球）、`crash()`／`playRecording()`（撞瓶錄音）、`playGrain()`（從錄音切出的單支球瓶聲）、`cheer()`／`aww()`（觀眾）、`fanfare()`（慶祝音樂）
- 特效：`emit()`（火花粒子）、`confettiBurst()`（彩帶）、`updateTrail()`（球的軌跡）、`celebrate()`（全倒／補中慶祝）、`update()` 裡的慢動作與鏡頭震動

## 音效來源

撞瓶錄音是 freesound 社群的「bowling strike」（#40456），以 base64 嵌在 `bowling.html` 的 `<script id="strike-mp3">` 裡，讓遊戲維持單一檔案。

## 可以優化的方向

- 撞瓶效果（目前正中頭瓶多半只倒 5–6 瓶，打進 1-3 號瓶縫才容易全倒）
- 滑動手感（球速與旋轉的換算比例）
- 球的重量選擇
- 球撞頭瓶後常會彈到半空中（慢動作特寫時特別明顯）
