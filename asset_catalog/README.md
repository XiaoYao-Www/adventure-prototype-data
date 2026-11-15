# asset_catalog（素材目錄索引）— v0.1（2026-11-16）

> 原始素材**不進本倉庫**。原檔位於工作區 `assets/`（使用者提供），本目錄只放
> 分類索引、雜湊、授權紀錄與使用規則，避免把受限素材包整個提交進 Git。

## 目前素材包

| pack id | 名稱 | 作者 | 檔案數 | 大小 | 授權 | 再散布 |
| --- | --- | --- | --- | --- | --- | --- |
| `mini_world_sprites` | 16x16 Mini World Sprites | Kurt Gabriel Sotelo（merchant-shade）、octoshrimpy | 207 | 約 1.4MB | **CC0-1.0**（已核對 itch.io 頁面） | 可 |
| `items16` | Items pack (x16) | Glionox | 1244 | 約 1.0MB | **自訂授權**（已核對 itch.io 頁面） | **不可**（不得再散布或轉售原始包） |

- 兩包皆可商用、可修改；授權查核日 2026-11-16，來源頁面：
  - https://merchant-shade.itch.io/16x16-mini-world-sprites
  - https://glionox.itch.io/items16
- 詳細授權摘要：`licenses/SUMMARY.md`；pack 紀錄：`packs/*.json`。

## 使用政策（重要）

- `mini_world_sprites`（CC0）：可嵌入遊戲、可修改、可再用於商業專案；署名非必要（建議）。
- `items16`：可嵌入遊戲使用與修改，但**不得再散布或轉售原始素材包**。
  - 因此：**原始 item 圖示不得放進公開 APK／公開內容包**；僅可在本地開發與內部測試建置使用。
  - 若要公開發布：改用其他可再散布的圖示、或取得作者書面許可、或改為不直接散布原始圖檔
    （例如僅在遊戲內顯示但避免可萃取？仍屬灰色，需明確授權才可）。
- 任何未完成授權核對的素材：標 `license_unverified`，不得發布。

## 分類（`catalog.json` / `generated/index_by_category.json`）

| category | 數量 | 說明 |
| --- | --- | --- |
| `terrain_ground` | 7 | 可鋪設地形圖塊（草、水、岸、崖、雪地等 16x16 圖塊表） |
| `nature` | 10 | 樹木、岩石、植物、仙人掌等 |
| `object` | 9 | 寶箱、告示、傳送門、墓碑等可放置物件 |
| `animal` | 8 | 豬、羊、雞、馬、海洋動物等 |
| `building` | 68 | 5 種配色（Cyan/Lime/Purple/Red/Wood）與敵方建築 |
| `character_soldier_melee` | 20 | 近戰士兵（含 4 種配色） |
| `character_soldier_ranged` | 15 | 遠程士兵（含 4 種配色） |
| `character_soldier_mounted` | 4 | 騎兵 |
| `character_worker` | 5 | 工人（含 4 種配色） |
| `character_champion` | 8 | 英雄／冠軍角色 |
| `monster_*` | 32 | 怪物（Demon/Dragon/Frostborn/GiantAnimals/Orc/Pirate/Slime/Undead） |
| `ui` | 4 | 介面元素 |
| `misc` | 12 | 雜項（石頭、傳送門等） |
| `template` | 3 | 空白模板／參考圖 |
| `preview` | 2 | 素材總覽預覽圖（非遊戲用） |
| `item_icon` | 1244 | Items16 的 16x16 道具圖示 |

- 每筆包含：`path`、`pack`、`category`、`tags`、`width/height`、`bytes`、`sha256`、
  `tile_grid`（16x16 格數）、`single_tile`、`sprite_sheet`、授權狀態與可否再散布。
- Mini World Sprites 的檔名本身有語意（Grass、Shore、CyanHouses…），已轉成 tags。
- **Items16 沒有語意檔名**（僅 `item1.png`…`item1244.png`），且包內無名稱對照表；
  本目錄只能標記為 `item_icon`，尚未做逐張語意分類。需要語意時請先看
  `generated` 連絡表（見下）再決定，不要盲目假設編號對應類型。

## 聯絡表（省 token 用，不進 Git）

- 工作區 `tools/asset_previews/mini_world_sprites.png`（207 張總覽，1024x832）。
- 工作區 `tools/asset_previews/items16.png`（1244 張總覽，1024x1248）。
- 對照索引：同目錄 `.json`（列優先：`i = row * columns + column`）。
- 產生方式（工作區工具）：`tools/make_contact_sheets.gd`。

## 重新產生目錄

```bash
node app_base_git/tools/build_asset_catalog.mjs \
  --assets /workspace/自動遊戲開發/assets \
  --out /workspace/自動遊戲開發/app_data_git/asset_catalog
```

只讀原始素材、不改檔；輸出可重現（含 sha256）。原始素材換版本時重跑即可。

## 檔案說明

- `catalog.json`：完整目錄（1451 筆，含雜湊與標籤）。
- `generated/index_by_category.json`：依分類分組的精簡索引。
- `packs/<pack>.json`：素材包來源／授權／檔案數。
- `licenses/SUMMARY.md`：授權摘要（人可讀）。

## 尚未完成

- Items16 逐張語意分類（需人工或作者提供對照表）。
- 素材實際接入遊戲（TileSet／角色動畫／道具圖示）；目前 APK 仍是 placeholder 美術。
- 授權頁面快照存檔（目前只記錄 URL 與查核日；發布前建議保存完整頁面）。
