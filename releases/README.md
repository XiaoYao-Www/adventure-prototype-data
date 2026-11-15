# releases（發布流程佔位）

> 狀態：`v0.1.0` 尚未發布任何內容包。此目錄目前只有本說明檔；發布產出物不進 Git。

## 預定流程（尚未自動化）

1. 確認 `docs/CONTENT_CHANGELOG.md`、`schema_version`、`min_game_version` 已更新。
2. 執行 `docs/CONTENT_SCHEMA.md` 第 13 節的完整驗證（JSON、schema、引用、ID、檔案格式）。
3. 重新計算 `manifest.json` 的 `sha256` 與 `size`，確認與實際檔案一致。
4. 打包內容包（格式待決策：ZIP／PCK／自訂；需先做技術驗證，不得先寫大型下載器）。
5. 授權與成人內容分流檢查：`license_unverified` 或未核准成人內容不得進入發布包。
6. 由授權人員人工發布；本倉庫不自動發布。
7. 建立不可變版本號與 tag，並記錄發布說明。

## 禁止

- 不提交建置產出、APK、內容包壓縮檔或快取。
- 不用 Git commit hash 當玩家端唯一版本識別。
- 不 force push、不刪除 tag／branch、不改 remote。
- 不在未完成授權審核時發布任何素材。

## 目前發布紀錄

無。
