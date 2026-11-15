# game-content — 遊戲內容包來源（v0.1.0）

本倉庫保存可由 `game-client` 載入的遊戲內容資料與可發布內容包來源。玩家存檔、私人設定與程式碼不在此。

> **狀態：`0.1.0` 內容包骨架。全部世界觀、地名、角色與對話皆為「暫定」，不是正史，不得當成已核准設定。**

## 目前包含

| 類型 | ID | 檔案 |
| --- | --- | --- |
| 地區 | `region_starting_001` | `world/regions/starting_region_001.json` |
| 地圖 | `map_start_village` | `world/maps/starting_village.json` |
| 地圖 | `map_forest_edge` | `world/maps/forest_edge.json` |
| NPC | `npc_village_elder_001` | `characters/npcs/npc_village_elder_001.json` |
| 道具 | `item_herb_001` | `items/item_herb_001.json` |
| 事件 | `event_elder_greeting_001` | `events/event_elder_greeting_001.json` |
| 事件 | `event_chest_herb_001` | `events/event_chest_herb_001.json` |

- 地圖 `map_start_village`（12×9）與 `map_forest_edge`（10×8）以 portal 雙向連接。
- 村長對話提供 1 個選項，設定 `flag_met_elder`。
- 森林寶箱提供藥草 `item_herb_001` ×1，並設定 `flag_chest_forest_herb_opened`。

## 載入順序（client 依此實作）

1. 讀取根目錄 `manifest.json`，確認 `manifest_schema`、`schema_version`、`min_game_version` 與自身版本相容。
2. 依 `files` 清單逐一載入：先驗證 `sha256` 與 `size`，再以對應 `schemas/` 驗證資料。
3. 再檢查引用完整性（event ↔ map/entity、choice.effects.item_id、region.map_ids、portal.target_map_id）。
4. 未知 `schema_version`、未知 effect type、未知欄位、雜湊不符 → 拒絕該內容並回報錯誤，不猜測執行，不覆寫已安裝舊版本。

## 目錄結構

```text
manifest.json
schemas/          JSON Schema v1（manifest/region/map/npc/item/event）
world/regions/    地區
world/maps/       地圖
characters/npcs/  NPC
items/            道具
events/           事件與對話
asset_catalog/    素材來源與授權索引（空）
optional_content/adult/  成人內容模組（空、預設不啟用）
releases/         發布流程說明（尚無發布）
docs/             資料契約、素材政策、變更紀錄
```

## 版本

- `content_id`：`base_world`
- `content_version`：`0.1.0`（SemVer）
- `schema_version`：`1`
- `min_game_version`：`0.1.0`
- `dependencies`：`[]`

版本規則、ID 規則與效果白名單見 `docs/CONTENT_SCHEMA.md`。

## 素材與授權

本輪**無任何第三方素材**，倉庫內沒有圖像或音訊檔案。`display_key`（例如 `npc_elder`、`chest`）只作為 client 產生 placeholder 的鍵值，不是資產路徑。授權紅線與 `license_unverified` 處理見 `docs/ASSET_POLICY.md`。

## 驗證

每次內容變更都必須重跑 `docs/CONTENT_SCHEMA.md`「驗證清單」的檢查，並確認 `manifest.json` 的 `sha256`／`size` 與實際檔案一致。未實際執行的項目不可標成通過。

## 本倉庫不做的事

- 不放大段劇情；只放可驗證的最小內容與清楚的資料契約。
- 不放程式碼、腳本、可執行檔、玩家資料、密鑰。
- 不放未授權素材或素材原始壓縮包。
- 不自動發布內容包。
