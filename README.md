# Research Tree

把研究過程畫成一棵樹的 Obsidian plugin。

做研究常被畫成一個循環：Motivation → Constraints → Objective → Method → Analyze，走不通就回頭改 Motivation。這個工具採用另一種看法：研究是一棵樹，Motivation 是根，往下長出 Constraint、Objective、Method、Experiment。走不通時只退回上一層換一條路，上面已經確認的決定不用推翻，走不通的分支也留著當紀錄。

每個節點就是一份 Markdown 筆記，樹的結構存在筆記的 frontmatter 裡。就算不用這個 plugin，筆記也都還在。

## 安裝

需要桌面版 Obsidian 1.4 以上（不支援手機版）。

**用 BRAT（會自動更新）**

1. 在 Obsidian 的社群外掛裡安裝並啟用 BRAT。
2. 指令面板執行「BRAT: Add a beta plugin for testing」，貼上 `https://github.com/bangyou0912/obsidian-research-tree`。
3. 到設定 → 社群外掛，啟用 Research Tree。

**手動安裝**

1. 到 [Releases](https://github.com/bangyou0912/obsidian-research-tree/releases) 下載最新版的 `main.js`、`manifest.json`、`styles.css`。
2. 放進你的 vault 的 `.obsidian/plugins/research-tree/`（資料夾自己建）。
3. 重新啟動 Obsidian，到設定 → 社群外掛啟用 Research Tree。

## 開始使用

1. 點左側功能列的網路狀圖示，或在指令面板執行「開啟研究樹」。
2. 第一次用可以先執行「建立範例樹」，會在 `研究樹/範例/` 建立 10 個虛構的節點，試完整個資料夾刪掉即可。
3. 要開始自己的題目，按工具列的「＋ Motivation」。

## 節點

| 類型 | 要回答的問題 | 底下可以放 |
|---|---|---|
| Motivation | 要解決什麼問題？為什麼這是真問題？ | Constraint |
| Constraint | 在什麼限制下解這個問題？ | Objective |
| Objective | 要量什麼 metric？越大還是越小越好？要贏過誰？ | Method |
| Method | 核心想法是什麼？為什麼它應該有效？ | Method（拆成子元件）或 Experiment |
| Experiment | 怎麼設定？結果是什麼？支持還是推翻假設？ | — |

每個節點有四種狀態：

- **探索中**：預設狀態。
- **主線** ★：目前選定要走的路。設為主線時，上面還在探索中的祖先會一併設為主線。
- **已驗證** ✓：有實驗結果支持。
- **已放棄** ✗：一定要寫一句原因。放棄的節點連同底下的子孫會變灰，但不會消失。

## 操作

| 動作 | 做法 |
|---|---|
| 新增子節點 | 游標移到節點上，按下方的「+」；或右鍵選單 |
| 開啟筆記 | 點節點，筆記會開在右邊 |
| 改狀態、放棄 | 在節點上按右鍵 |
| 換 parent | 把節點拖到另一個節點上（類型不合會被拒絕） |
| 只調整位置 | 把節點拖到空白處，底下的子樹會跟著移動 |
| 掛資源 | 從檔案總管把 PDF 或筆記拖到節點上，節點會顯示 📎 數量 |
| 平移畫布 | 拖曳空白處，或直接滾動 |
| 縮放 | 工具列的「－」「＋」，或 Cmd／Ctrl＋滾輪、觸控板捏合 |
| 還原自動排版 | 工具列的「重設排版」 |

節點右上角的橘點：實心代表筆記還是空的，空心代表有內容但還缺必填欄位（游標移上去會列出缺哪些）。

## AI 擷取欄位（選用）

你只要在筆記裡用自己的話寫，AI 會把重點擷取成 frontmatter 欄位，例如 Objective 的 `metric`、`direction`、`baseline`，或 Experiment 的 `result`、`verdict`。筆記沒寫到的欄位會留空，不會蓋掉你自己填的值。

- 單一節點：右鍵 →「AI 擷取欄位」。
- 整棵樹：工具列的「AI 擷取欄位」，只處理有內容但還缺必填欄位的節點。

**前提**：這台電腦要裝好 [Claude Code](https://claude.com/claude-code) 並登入。plugin 會呼叫本機的 `claude` 指令，用的是你自己的 Claude 訂閱額度，不需要 API key。沒裝的話只有這個功能不能用，其他功能不受影響。

如果 plugin 找不到 `claude`，到設定 → Research Tree 填入完整路徑：

- macOS：通常是 `~/.local/bin/claude`。
- Windows 原生安裝版：通常是 `C:\Users\你的帳號\.local\bin\claude.exe`。
- Windows 用 npm 安裝：通常是 `C:\Users\你的帳號\AppData\Roaming\npm\claude.cmd`。

**Windows 使用者注意**：Claude Code 必須裝在 Windows 本身。裝在 WSL 裡的 Claude Code，Obsidian 呼叫不到。

**隱私**：執行 AI 擷取時，該節點的筆記內容，以及它上層節點的標題與欄位，會經由 Claude Code 送到 Anthropic。不執行 AI 擷取就不會送出任何資料。還沒發表的研究內容請自行判斷要不要用。

## 資料存在哪裡

- 節點：`研究樹/` 資料夾（可在設定更改）裡，frontmatter 有 `type` 的筆記。
- 結構與狀態：筆記 frontmatter 的 `type`、`parent`、`status`、`reason`、`sources`。
- 手動調整過的節點位置與設定：`.obsidian/plugins/research-tree/data.json`。

```yaml
---
type: method
parent: "[[降低 LLC miss rate]]"
status: abandoned
reason: "存取樣式不規則，prefetch 幾乎猜不中"
sources:
  - "[[某篇論文.pdf]]"
---
```

## 已知限制

- 只支援桌面版，介面只有繁體中文。
- Windows 上的 AI 擷取還沒有實際測過（程式有處理原生安裝版與 npm 版，但沒有在 Windows 上跑過）。遇到問題請把錯誤訊息回報給我。
- 「把資源拖到節點上」用到 Obsidian 的非公開 API，Obsidian 改版後可能失效。
- 還沒有匯出論文大綱的功能。

## 回報問題

這是早期版本，歡迎直接告訴我哪裡不好用，特別是：五種節點套不套得進你的研究、哪些操作讓你卡住。
