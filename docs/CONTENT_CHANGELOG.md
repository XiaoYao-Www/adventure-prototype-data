# 內容包變更紀錄

格式參考 Keep a Changelog；日期為 Asia/Taipei。內容版本遵循 SemVer（規則見 `docs/CONTENT_SCHEMA.md` 第 12 節）。

## [0.1.1] - 2026-11-16

### 變更

- 村長對話新增一句提示（森林邊緣路況），用於驗證內容自動更新流程。
- `manifest.json` `content_version` 0.1.0 → 0.1.1（schema_version 仍為 1，相容 min_game_version 0.1.0）。

### 驗證

- 內容包驗證器通過（雜湊、引用、manifest）。

## [Unreleased]

### 素材目錄

- 建立 `asset_catalog/`：Mini World Sprites（CC0-1.0）與 Items16（自訂授權、禁止再散布）的
  分類、雜湊、授權摘要與使用政策；1451 張 16x16 PNG。
- 原始素材不進倉庫；工作區 `assets/` 為來源，索引由 `game-client/tools/build_asset_catalog.mjs` 產生。
- **未變更**內容包 `manifest.json` 與 `content_version`（素材尚未接入遊戲）。

## [0.1.0] - 2026-11-16

### 新增

- 內容包骨架：`manifest.json`（`manifest_schema` 1、`content_id` `base_world`、`content_version` `0.1.0`、`schema_version` 1、`min_game_version` `0.1.0`、`dependencies` `[]`），`files` 依 path 排序並含真實 sha256／size。
- JSON Schema v1：`manifest`、`region`、`map`、`npc`、`item`、`event`。
- 文件：`docs/CONTENT_SCHEMA.md`、`docs/ASSET_POLICY.md`、根目錄 `README.md`、`AGENTS.md`、`.gitignore`。
- 地區：`region_starting_001`（含兩張地圖）。
- 地圖：`map_start_village`（12×9）、`map_forest_edge`（10×8）；雙向 portal。
- 角色：`npc_village_elder_001`（村長）。
- 道具：`item_herb_001`（藥草）。
- 事件：`event_elder_greeting_001`（1 個選項 → `set_flag` `flag_met_elder`）、`event_chest_herb_001`（1 個選項 → `give_item` + `set_flag` `flag_chest_forest_herb_opened`）。
- 佔位目錄說明：`asset_catalog/`、`optional_content/adult/`、`releases/`。

### 說明

- 全部世界觀、地名、角色與對話皆為**暫定**，不是正史。
- 本輪無任何第三方素材；`display_key` 由 client 以程式內 placeholder 呈現。
- 成人內容模組為空、預設不啟用。
- 效果白名單只有 `set_flag` 與 `give_item`。

### 驗證

- 以 node v24 內建模組執行 JSON 解析、schema 比對、引用完整性、ID 唯一性、tiles 幾何、manifest 雜湊與檔案格式檢查（清單見 `docs/CONTENT_SCHEMA.md` 第 13 節）；結果記於當次交接報告。
- **未檢查**：Godot client 實際載入與遊戲內互動（由 `game-client` 端整合測試）；Android 實機測試；素材授權（本輪無素材）。

### 已知限制

- 無戰鬥、任務、商店、敵人、技能、存檔格式、旗標定義檔。
- `repeatable: false` 的只觸發一次需 client 依旗標判斷。
- 地圖與事件僅足夠驗證載入與基本互動，不足以構成完整遊戲循環。

## [0.2.0] - 2026-11-16

### 新增

- 地圖擴充為 5 張：起始村落、村落廣場、森林邊緣、森林深處、湖畔（雙向 portal）。
- NPC 6 名：村長、雜貨商、衛兵、獵人、隱者、漁夫。
- 道具 6 種：藥草、野莓、木材、石塊、湖魚、舊硬幣。
- 事件 14 個：對話、寶箱、告示、水井、神社、市場等互動。

### 驗證

- 內容包驗證器 CONTENT OK（5 地圖、6 NPC、6 道具、14 事件）；client 336 項測試通過。
