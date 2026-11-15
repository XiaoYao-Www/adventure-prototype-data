# game-content 倉庫指令（AGENTS.md）

本檔案是 `game-content` 倉庫內的主要工作區指令。此倉庫保存遊戲內容與可發布內容包來源，不含遊戲本體程式。不得假設父目錄的 `AGENTS.md` 一定會自動載入。

## 定位與範圍

- 本專案是視覺優先的 2D 格子探索 RPG；內容必須支援場景、圖像、物件與互動，不得退化成純文字內容堆積。
- 本倉庫只放：地圖、地區、角色、敵人、道具、技能定義、事件、任務、世界狀態、資產索引、授權紀錄與內容包來源。
- 不放：遊戲執行邏輯、任意腳本、原生可執行檔、玩家存檔、密鑰、未授權素材原始包、未審核成人內容。

## 不可違反的規則

- DSH 在 Unraid 上只使用 CPU，不得要求或嘗試啟用 GPU。
- 不使用未經核准的付費 API、雲端、插件或依賴；不自行安裝 npm／Godot 外掛。
- 兩個倉庫分離：`game-client` 保存程式，`game-content` 保存內容。不合併、不新增第三個 Git 倉庫。
- 不覆蓋使用者變更，不 reset／clean，不改 remote，不 force push，不自動發布。
- 不提交密鑰、玩家存檔、未授權素材原始包或私人資料。
- 素材來源、作者、版本與完整授權條款必須可追溯；不確定授權時禁止發布（標 `license_unverified`）。
- 所有成人內容必須可選、角色明確成年、互動自願，並遵守適用法律與平台政策；成人包預設不啟用。
- 內容資料不得包含或執行任意程式碼；所有內容需符合正式 schema。
- 內容 ID 一經發布不得更名、重用或改作不同意義。
- 本倉庫不放可執行邏輯：驗證腳本放在系統暫存區或 `game-client` 工具目錄，不進內容倉庫。

## 開始工作前必讀

共享提示詞位於工作區 `PROMPTS/`；從本倉庫根目錄的相對路徑為 `../PROMPTS/`。若此路徑不存在，先搜尋工作區的 `PROMPTS/`；找不到才回報缺少文件，不要自行重建另一套規範。

1. `../PROMPTS/05_CONTENT_REPO.md`
2. `../PROMPTS/06_ARCHITECTURE_AND_DATA_CONTRACTS.md`
3. `../PROMPTS/08_ART_ASSET_PIPELINE.md`
4. `../PROMPTS/10_NARRATIVE_AND_WORLD_STATE.md`
5. `../PROMPTS/11_ADULT_CONTENT_AND_MODULARITY.md`
6. `../PROMPTS/15_SECURITY_LICENSES_AND_PRIVACY.md`
7. `../PROMPTS/20_CONTENT_AUTHORING_AND_REVIEW.md`
8. 本倉庫 `docs/CONTENT_SCHEMA.md`、`docs/ASSET_POLICY.md`、`docs/CONTENT_CHANGELOG.md`

## 目錄與資料契約

```text
manifest.json              # 內容包清單（含 sha256/size）
schemas/                   # JSON Schema v1（manifest/region/map/npc/item/event）
world/regions/             # 地區定義
world/maps/                # 地圖（tiles、legend、spawn、entities、portals）
characters/npcs/           # NPC 定義
items/                     # 道具定義
events/                    # 事件／對話定義
asset_catalog/             # 素材來源與授權索引（目前為空）
optional_content/adult/    # 成人內容模組（目前為空、預設不啟用）
releases/                  # 發布流程說明（尚無發布）
docs/                      # 契約、素材政策、變更紀錄
```

- 資料契約與驗證清單：`docs/CONTENT_SCHEMA.md`。schema 檔與文件衝突時，以 `schemas/` 為準並回報修正。
- 效果白名單只有 `set_flag` 與 `give_item`；未知效果一律拒絕載入。
- `manifest.json` 的 `files` 只列資料 JSON，不含 `schemas/`、`docs/`、README 與 manifest 本身。

## 目前狀態（v0.1.0）

- 首個可驗證內容包骨架：1 個地區、2 張地圖、1 名 NPC、1 個道具、2 個事件。
- 全部世界觀、地名、角色與對話皆為**暫定**，不是正史。
- 無任何第三方素材；`display_key` 由 client 以程式內 placeholder 呈現。
- 成人內容模組為空、預設不啟用。

## 任務完成時

驗證 schema、ID、引用、版本、雜湊與授權；檢查 `git status`／diff；更新 `docs/CONTENT_CHANGELOG.md` 與發布說明。不可將未核准素材放入公開內容包。未執行的測試必須標為「未檢查」，不可寫成已通過。
