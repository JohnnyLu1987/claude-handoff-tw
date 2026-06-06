# claude-handoff-tw

[English](README.md) | 繁體中文

**再也不怕 AI 對話 session 斷掉、忘記進度。**

兩個 Claude Code 技能（skill），自動記錄決策過程、踩過的坑、測量數據和下一步行動——讓下一個 session 從你停下來的地方無縫接手。不再浪費 20-40% 的時間重新發現已知的事情。

## 技能介紹

### `/handoff`
擷取 session 脈絡——下一個 session 繼續探索。

當工作暫停、或 context 快用完時執行（約 75%）。完整挖掘你的對話紀錄、蒐集 git 狀態、驗證輸出，並產生一段可直接貼到下一個 session 的接手提示。

### `/handoffplan`
擷取脈絡 + 撰寫分階段計畫——下一個 session 直接執行。

研究階段完成、準備動手建構時執行。產生一個 HANDOFF 檔案 + 一個 PLAN 檔案，內含分階段實作步驟、相依關係、反目標（Anti-Goals）和成功標準。下一個 session 收到的接手提示會說「從 Phase 1 開始執行。直接建構。」

## `/handoff` vs `/handoffplan` 比較

| | `/handoff` | `/handoffplan` |
|---|---|---|
| **適用時機** | 工作暫停中 | 研究完成、準備建構 |
| **寫入內容** | Handoff 檔案 | Handoff + 分階段計畫 |
| **下個 session** | 讀取 handoff，繼續探索 | 讀取計畫，開始撰寫 Phase 1 |

## 你會得到什麼

每次 handoff 都會擷取：
- **目標** — 我們在解決什麼、為什麼
- **現況** — 目前狀態（15-25 條）
- **嘗試過的方法** — 每個方法的時序紀錄（最有價值的段落）
- **關鍵決策** — 選擇了什麼、拒絕了什麼
- **證據與數據** — 真實數字，不是摘要
- **使用者回饋** — 偏好、修正、語氣
- **下一步** — 有序的後續行動
- **快速啟動** — 給下一個 session 的確切指令

## 本 Fork 的修改

這是針對**繁體中文**使用者的修改版 fork。

1. **輸出資料夾改變**：從 `plans/handoffs/` 改為 `plan/`——handoff 和計畫檔案現在存到目前工作目錄下的 `plan/` 資料夾（不存在時自動建立）。
2. **雙語接手提示**：session 結束時的「下一個 session」接手提示，會自動依對話語言輸出繁體中文或英文，無需任何設定。

## 安裝

在 Claude Code 中執行：

```
/plugin marketplace add JohnnyLu1987/claude-handoff-tw
```

## 使用方式

- 工作暫停或 context 快滿時：輸入 `/handoff`
- 研究完成、要開始建構時：輸入 `/handoffplan`

接手提示（PASTE THIS INTO YOUR NEXT SESSION）會自動以**繁體中文**輸出（當你用中文對話時）。

## 來源與著作標註

- **原始專案：** [REMvisual/claude-handoff](https://github.com/REMvisual/claude-handoff)（MIT 授權）
- **參考的上游 fork：** [willseltzer/claude-handoff](https://github.com/willseltzer/claude-handoff)
- **本 fork 維護者：** 呂宗祐（Johnny Lu）— [github.com/JohnnyLu1987/claude-handoff-tw](https://github.com/JohnnyLu1987/claude-handoff-tw)

本專案採 MIT 授權——詳見 [LICENSE](LICENSE)。
