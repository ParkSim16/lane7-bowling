# Lane 7 保齡球

第一人稱 3D 保齡球遊戲，單一 HTML 檔，使用 three.js 繪圖、cannon-es 做物理模擬（皆由 CDN 載入）。

## 玩法

- **丟球**：按住畫面往上滑，放開出手；滑得越快球速越快
- **瞄準**：斜著滑，球道上會出現瞄準線
- **曲球**：滑出弧線，球會在球道後段轉彎
- **站位**：← → 鍵或畫面兩側按鈕

標準十格計分（第十格最多三球），最佳分數存在瀏覽器 localStorage。

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

## 可以優化的方向

- 撞瓶效果（目前正中頭瓶多半只倒 5–6 瓶，打進 1-3 號瓶縫才容易全倒）
- 滑動手感（球速與旋轉的換算比例）
- 球的重量選擇、多人輪流、音效與特效
