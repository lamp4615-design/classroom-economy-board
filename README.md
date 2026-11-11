# 班級經濟

課堂用的班級經濟系統：老師管理計分，學生可以投資、闖關賺取班級幣。純前端靜態網頁，不需要建置；資料由 Google Apps Script 後端存取，並同步到 Google 試算表。

網站（GitHub Pages）：https://lamp4615-design.github.io/classroom-economy-board/

## 資料夾結構

```
classroom-economy-board/
├─ index.html          入口頁，連到下面三個功能（全部人）
├─ scoreboard.html     班級計分板＋大螢幕模式（老師）
├─ invest.html         投資市場：K 線、均線、成交量、多標的買賣（學生）
├─ game.html           闖關挑戰（學生）
├─ docs/               說明文件
│  ├─ scoreboard.md         計分板與大螢幕模式
│  ├─ teacher-dashboard.md  教師後台
│  └─ backend.md            後端設定、老師工具、常見問題
└─ local/              ⚠ 只在老師電腦，不上 GitHub（.gitignore 排除）
   ├─ teacher.html          教師後台
   ├─ Code.gs               後端程式碼（貼進 Apps Script）
   └─ archive/              舊版檔案，只留存不再使用
```

上面四個 `.html` 是公開網站，學生用的網址就是它們的檔名，**不要搬動或改名**。

`local/` 含學生資料入口與後端細節，只存在這台電腦。請自行備份整個 `local/` 資料夾（例如複製到雲端硬碟）。

## 怎麼用

| 我要… | 去哪裡 | 說明 |
| --- | --- | --- |
| 上課計分、管理學生 | `scoreboard.html` | [計分板與大螢幕模式](docs/scoreboard.md) |
| 讓學生自己投資 | 把 `invest.html` 網址給學生 | 見下方「投資市場」 |
| 看誰需要關心、輔導配對、登記月考 | 用瀏覽器開 `local/teacher.html` | [教師後台](docs/teacher-dashboard.md) |
| 第一次設定或更新後端 | 把 `local/Code.gs` 貼進 Apps Script | [後端設定](docs/backend.md) |

## 投資市場

提供乙太幣、黃金、台灣加權指數三個標的，使用真實行情：

- 乙太幣：CoinGecko
- 黃金、台灣加權指數：Yahoo Finance

行情由後端 Apps Script 定期抓取並快取（每 15 分鐘更新一次），買賣結算也在後端完成。

## 改程式時要一起改的地方

- **判讀規則**（誰是「需關心／留意」）：`local/teacher.html` 的 `judge()` 和 `local/Code.gs` 的 `judgeStudent_`／`classContext_` 要一致，每週摘要信與每日狀態紀錄用的是後端那份。
- **存取金鑰**：各頁面的 `API_KEY` 必須和 Apps Script 指令碼屬性 `CE_API_KEY` 完全一致。
- **後端改完**一定要在 Apps Script 重新部署「新版本」，網頁才會用到新程式。
