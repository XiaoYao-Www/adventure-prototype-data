# game-content 倉庫指令

你正在維護遊戲內容與發布資產來源。這份檔案會在此 Git 倉庫內作為主要工作區指令，因此不可假設父目錄的 `AGENTS.md` 一定會自動載入。

## 不可違反的共通規則

- 本專案是視覺優先的 2D 格子探索 RPG；內容必須支援場景、圖像、物件與互動，不要退化成純文字內容堆積。
- DSH 在 Unraid 上只使用 CPU，不得要求 GPU。
- 不使用未經核准的付費 API、雲端、插件或依賴。
- 兩個倉庫分離：`game-client` 保存程式，本倉庫 `game-content` 保存內容。
- 不覆蓋使用者變更，不 reset／clean，不改 remote，不 force push，不自動發布。
- 不提交密鑰、玩家存檔、未授權素材原始包或私人資料。
- 素材來源、作者、版本與完整授權條款必須可追溯；不確定授權時禁止發布。
- 所有成人內容必須可選，角色明確為成年人，互動自願，並遵守適用法律與平台政策。
- 內容資料不得包含或執行任意程式碼；所有內容需符合正式 schema。

## 本倉庫責任

只負責地圖、地區、角色、敵人、道具、技能定義、事件、任務、故事狀態、資產索引、授權紀錄與可發布內容包來源。不得放入遊戲本體程式、玩家存檔或未核准素材。

## 開始工作前讀取

1. `docs/CONTENT_SCHEMA.md`
2. `docs/ASSET_POLICY.md`
3. `docs/CONTENT_CHANGELOG.md`（若存在）
4. 工作區共享提示詞中的 `PROMPTS/05_CONTENT_REPO.md`、`06_ARCHITECTURE_AND_DATA_CONTRACTS.md`、`08_ART_ASSET_PIPELINE.md`、`10_NARRATIVE_AND_WORLD_STATE.md`、`11_ADULT_CONTENT_AND_MODULARITY.md`、`15_SECURITY_LICENSES_AND_PRIVACY.md`。

若共享提示詞路徑不存在，先搜尋工作區的 `PROMPTS/`；找不到才回報缺少文件，不要自行重建另一套規範。

## 任務完成時

驗證 schema、ID、引用、版本、雜湊與授權；檢查 Git diff；更新內容 changelog 與發布說明。不可將未核准素材放入公開內容包。
