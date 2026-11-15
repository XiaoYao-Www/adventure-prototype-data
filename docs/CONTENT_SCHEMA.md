# 內容資料契約 v1（v0.1.0）

> 狀態：草案，隨內容包 `0.1.0` 使用。全部世界觀內容為**暫定**。
> `manifest_schema = 1`、`schema_version = 1`、`min_game_version = "0.1.0"`。
> 本文件是人類可讀整理；與 `schemas/*.schema.json` 衝突時，以 schema 檔為準並回報修正。

## 1. 範圍

- 只描述 `manifest.json` `files` 清單中的資料 JSON。`schemas/`、`docs/`、README 與 `manifest.json` 本身不列入。
- 內容不得包含或執行任意程式碼；client 只依固定載入規則與效果白名單解析。
- 所有資料檔案為 UTF-8、無 BOM，一個檔案一個根物件，根物件含 `schema_version: 1`（`manifest.json` 用 `manifest_schema` 與 `schema_version`）。
- 本契約不含玩家存檔格式；存檔與內容版本分離。

## 2. 目錄與對應 schema

| 目錄 | schema | 說明 |
| --- | --- | --- |
| `world/regions/*.json` | `region.schema.json` | 地區 |
| `world/maps/*.json` | `map.schema.json` | 地圖 |
| `characters/npcs/*.json` | `npc.schema.json` | NPC |
| `items/*.json` | `item.schema.json` | 道具 |
| `events/*.json` | `event.schema.json` | 事件／對話 |
| `manifest.json`（根） | `manifest.schema.json` | 內容包清單；不列入自身 `files` |

## 3. `manifest.json` 欄位

| 欄位 | 型別 | 必填 | 規則 |
| --- | --- | --- | --- |
| `manifest_schema` | integer | 是 | 固定 `1`（manifest 格式版本） |
| `content_id` | string | 是 | `^[a-z][a-z0-9_]{2,63}$`；本包為 `base_world` |
| `content_version` | string | 是 | SemVer `MAJOR.MINOR.PATCH`；本包 `0.1.0` |
| `schema_version` | integer | 是 | 固定 `1`（資料契約版本） |
| `min_game_version` | string | 是 | SemVer；本包 `0.1.0` |
| `dependencies` | array | 是 | 本包 `[]`；元素為 `{content_id, content_version}` 物件 |
| `files` | array | 是 | 至少 1 筆；元素為 `{path, sha256, size}` |

### `files` 規則

- 只列資料 JSON（`world/`、`characters/`、`items/`、`events/` 下的 `.json`），不含 `schemas/`、`docs/`、README、`manifest.json` 本身。
- `path`：倉庫根相對路徑、以 `/` 分隔、不得為絕對路徑、不得含 `..`、長度上限 200。
- 依 `path` 做 UTF-8 位元組字典序**嚴格遞增**排序；`path` 不得重複。
- `sha256`：該檔案完整內容的 SHA-256，64 位小寫十六進位。
- `size`：該檔案位元組數（正整數）。
- 雜湊與大小必須由實際檔案重新計算後寫入，不得保留 `REPLACE` 之類佔位字串。
- `dependencies` 目前為空；未來若新增，client 需先解析依賴再載入本包。

## 4. 共同規則

- 所有 ID 符合 `^[a-z][a-z0-9_]{2,63}$`（小寫 snake_case），前綴表意（見第 11 節）；全域唯一。
- 座標為 `[x, y]` 整數陣列，長度恰為 2；原點在左上，x 向右、y 向下，`0` 起算。
- `name` 長度 1–40 字元；本版所有地區、地圖、NPC 名稱須含「暫定」字樣（世界觀未定案）。
- 不得出現 schema 以外的欄位（`additionalProperties: false`）；新增欄位需提升 `schema_version`。
- 不使用未知 tile 字元、未知 effect type、未知 `schema_version`；client 一律拒絕並回報錯誤。

## 5. `region` 欄位

| 欄位 | 型別 | 必填 | 規則 |
| --- | --- | --- | --- |
| `schema_version` | integer | 是 | 固定 `1` |
| `id` | string | 是 | `^region_[a-z0-9_]{2,63}$` |
| `name` | string | 是 | 1–40 字元；含「暫定」 |
| `map_ids` | array[string] | 是 | 1–32 筆、不重複；每筆 `^map_[a-z0-9_]{2,63}$` 且必須存在對應 map 檔案 |

## 6. `map` 欄位

| 欄位 | 型別 | 必填 | 規則 |
| --- | --- | --- | --- |
| `schema_version` | integer | 是 | 固定 `1` |
| `id` | string | 是 | `^map_[a-z0-9_]{2,63}$` |
| `name` | string | 是 | 1–40 字元；含「暫定」 |
| `size` | object | 是 | `{width, height}` 整數 4–64 |
| `tile_size` | integer | 是 | 固定 `16`（像素） |
| `tiles` | array[string] | 是 | 列數 = `height`；每列長度 = `width`；字元 ⊆ legend key |
| `legend` | object | 是 | 1–5 個 key；key 限 `# . ~ p t`；值為 `{tile, walkable}` |
| `spawn` | object | 是 | `{grid}`；可通行且在範圍內 |
| `entities` | array | 是 | 最多 32 筆；元素見下 |
| `portals` | array | 是 | 最多 8 筆；元素見下 |

### tiles 與 legend

- `legend` 的 key 只允許 `#` `~` `p` `t` `.`；`tiles` 每一列只能使用 legend 中出現過的字元。
- `legend[key].tile` 只允許：`grass`、`path`、`wall`、`water`、`tree`、`floor`。
- `walkable` 與 tile 對應必須一致：`grass`／`path`／`floor` = `true`；`wall`／`water`／`tree` = `false`。
- 不得使用未在 legend 宣告的字元；不得宣告 legend 卻未被 tiles 使用以外的額外字元（本版 key 上限 5）。

### entity

| 欄位 | 型別 | 必填 | 規則 |
| --- | --- | --- | --- |
| `id` | string | 是 | snake_case；NPC 用 `npc_` 前綴，物件用 `chest_`／`object_` 等表意前綴；實體之間全域唯一 |
| `type` | string | 是 | `npc` 或 `object` |
| `grid` | `[x,y]` | 是 | 在範圍內且該格 `walkable = true` |
| `display_key` | string | 是 | `^[a-z][a-z0-9_]{1,31}$`；只作 client placeholder 鍵值，不是資產路徑 |
| `interaction` | object | 是 | `{kind, event_id}` |

- `interaction.kind` 只允許 `"event"`；`event_id` 必須存在，且該事件 `trigger` 必須指回本地圖與本 entity id。
- `type = npc` 的 entity id 必須有對應 `characters/npcs/<id>.json`。**NPC 放置實例與 NPC 定義共用同一個 ID**（同一個角色，只是地圖上的出現位置），這是唯一允許的跨類型同 ID 情形。
- `type = object` 的 entity 只需事件存在，不需要另外的物件定義檔；其 id 不得與任何地圖／地區／NPC／道具／事件／portal ID 衝突。
- 所有 entity id 在全部地圖之間全域唯一，同一地圖內也不得重複。

### portal

| 欄位 | 型別 | 必填 | 規則 |
| --- | --- | --- | --- |
| `id` | string | 是 | `^portal_[a-z0-9_]{2,63}$`；全域唯一 |
| `grid` | `[x,y]` | 是 | 在範圍內且該格 `walkable = true` |
| `target_map_id` | string | 是 | `^map_[a-z0-9_]{2,63}$` 且必須存在 |
| `target_grid` | `[x,y]` | 是 | 在**目標地圖**範圍內、該格 `walkable = true`，且不得等於目標地圖任一個 portal 的 `grid`（避免落地即反向觸發） |

## 7. `npc` 欄位

| 欄位 | 型別 | 必填 | 規則 |
| --- | --- | --- | --- |
| `schema_version` | integer | 是 | 固定 `1` |
| `id` | string | 是 | `^npc_[a-z0-9_]{2,63}$` |
| `name` | string | 是 | 1–40 字元；含「暫定」 |
| `role` | string | 是 | 1–40 字元 |
| `short_description` | string | 是 | 1–120 字元 |
| `is_adult` | boolean | 是 | 角色是否為成年人；成人內容角色必須 `true` |
| `speech_style` | string | 是 | 1–80 字元；一句話描述說話方式 |

- 本版 NPC 不涉及成人內容；`is_adult` 僅作為角色資料欄位。

## 8. `item` 欄位

| 欄位 | 型別 | 必填 | 規則 |
| --- | --- | --- | --- |
| `schema_version` | integer | 是 | 固定 `1` |
| `id` | string | 是 | `^item_[a-z0-9_]{2,63}$` |
| `name` | string | 是 | 1–40 字元 |
| `category` | string | 是 | 本版允許 `consumable`、`material`、`key`；`0.1.0` 只使用 `consumable` |
| `description` | string | 是 | 1–200 字元；不得承諾尚未實作的遊戲效果 |
| `stack_limit` | integer | 是 | 1–9999 |

## 9. `event` 欄位

| 欄位 | 型別 | 必填 | 規則 |
| --- | --- | --- | --- |
| `schema_version` | integer | 是 | 固定 `1` |
| `id` | string | 是 | `^event_[a-z0-9_]{2,63}$`；全域唯一 |
| `kind` | string | 是 | 本版只允許 `"dialogue"` |
| `trigger` | object | 是 | `{map_id, entity_id}`；entity 必須存在於該 map，且其 `interaction.event_id` = 本事件 id |
| `repeatable` | boolean | 是 | 可否重複觸發；`false` 的「只一次」由 client 依世界旗標判斷 |
| `lines` | array | 是 | 1–6 句；本版內容規範最多 3 句短句 |
| `choices` | array | 否 | 省略代表純對話；有則 1–4 個 |

### lines 元素

| 欄位 | 型別 | 必填 | 規則 |
| --- | --- | --- | --- |
| `speaker_id` | string | 是 | `"player"` 或存在的 `npc_*` id |
| `text` | string | 是 | 1–120 字元；短句，不要長篇小說 |

### choices 元素

| 欄位 | 型別 | 必填 | 規則 |
| --- | --- | --- | --- |
| `id` | string | 是 | `^[a-z][a-z0-9_]{1,31}$`；同一事件內唯一 |
| `text` | string | 是 | 1–80 字元；選項文字 |
| `effects` | array | 是 | 1–4 筆；只能是第 10 節白名單效果 |

## 10. 效果白名單（唯二允許）

| type | 參數 | 型別／規則 | 錯誤處理 |
| --- | --- | --- | --- |
| `set_flag` | `flag` | string `^flag_[a-z0-9_]{2,63}$` | 未知型別或缺參數 → client 拒絕整包並回報 |
| | `value` | boolean | 同上 |
| `give_item` | `item_id` | string `^item_[a-z0-9_]{2,63}$`，必須存在於 `items/` | 道具不存在 → 拒絕載入 |
| | `qty` | integer 1–99 | 非正整數或超上限 → 拒絕載入 |

- 本版**明確不支援**：`remove_item`、`change_relationship`、`start_quest`、`unlock_location`、`start_encounter`、`show_dialogue`。這些是 `PROMPTS/06` 的長期候選，若要啟用必須提升 `schema_version` 並由 client 實作。
- 不接受自訂／任意 effect type；不接受在內容中嵌入程式碼或路徑字串。
- 效果執行順序：依 `effects` 陣列順序；同一 choice 允許 1 個 `give_item` + 1 個 `set_flag`（本版實際用法）。

## 11. ID 與命名規則

- 格式：`^[a-z][a-z0-9_]{2,63}$`，小寫 snake_case，不用大寫、連字號、空白或中文。
- 前綴表意：

| 類型 | 前綴 | 範例 |
| --- | --- | --- |
| 地圖 | `map_` | `map_start_village` |
| 地區 | `region_` | `region_starting_001` |
| NPC | `npc_` | `npc_village_elder_001` |
| 道具 | `item_` | `item_herb_001` |
| 事件 | `event_` | `event_elder_greeting_001` |
| 旗標 | `flag_` | `flag_met_elder` |
| 傳送點 | `portal_` | `portal_start_to_forest` |
| 物件實體 | `chest_`／`object_` | `chest_forest_herb_001` |
| 選項 | 一般 snake_case | `accept`、`take_herb` |

- 內容物件（map／region／npc／item／event／portal）的 ID **全域唯一**（跨類型、跨檔案），不得重複；已發布 ID 不得改名、重用或改作不同意義。需要替換時新增 ID，並提供遷移／相容處理。
- 例外：地圖上的 NPC 放置實例 `entity.id` 等於該 NPC 的定義 ID（同一個角色）。`entity.id` 在實體之間（跨所有地圖）仍必須唯一，物件實體 ID 也不得與其他內容 ID 衝突。
- 檔名 = JSON `id` + `.json`（本版約定；實際路徑以 manifest 為準）。
- 旗標目前沒有獨立定義檔；client 以字串保存。新增旗標前先查是否已有相同語意。

## 12. 版本與相容性規則

- `content_version`：內容包 SemVer。只新增內容資料 → `MINOR`；修正錯字／數值 → `PATCH`；刪除或改義既有 ID → `MAJOR`。
- `schema_version`：資料契約版本（整數）。以下變更必須提升並同步 client：新增／移除必填欄位、改欄位語意、新增 effect type、改 tile／category 白名單、改排序規則。client 看到不認識的 `schema_version` 必須拒絕載入。
- `min_game_version`：可讀本包的最低 client 版本。client 較低 → 提示不相容並保留舊內容，不覆寫。
- `manifest_schema`：manifest 格式版本，本版固定 `1`。
- 內容更新不得改寫玩家存檔；存檔遷移需另立版本化程序。
- 不得用 Git commit hash 當玩家端唯一版本識別；發布版本使用不可變的正式版本號。

## 13. 驗證清單（每次內容變更必跑）

1. **JSON 可解析**：所有 `.json` 檔案可 `JSON.parse`；`manifest.json` 符合 `manifest.schema.json`；每個資料檔符合對應 schema（含 required、型別、`additionalProperties: false`）。
2. **地圖幾何**：`tiles` 列數 = `height`、每列長度 = `width`；所有字元在 `legend` 內；`legend.tile` 在白名單；`walkable` 與 tile 一致。
3. **範圍與可通行**：`spawn`、每個 `entity.grid`、每個 `portal.grid` 在範圍內且 walkable；`portal.target_grid` 在目標地圖可通行，且不等於目標地圖任一 portal 的 `grid`。
4. **引用完整性**：`event.trigger.map_id`／`entity_id` 存在且互相對應；`choices.effects.item_id` 存在；`region.map_ids` 存在；`portal.target_map_id` 存在；`speaker_id` 為 `player` 或存在的 NPC。
5. **ID**：全域唯一；前綴與命名規則符合；同一事件內 choice id 唯一；同一地圖內 entity id、portal id 唯一。
6. **manifest 一致性**：`files` 與實際資料檔集合完全相同；每筆 `sha256`、`size` 與檔案一致；依 `path` 嚴格遞增排序；無重複、無缺少、無多餘；無 `REPLACE` 佔位。
7. **檔案格式與安全**：只有 `.json` 與 `.md`（加上 `.gitignore`）；無 `.gd`／`.dll`／`.exe`／`.sh` 等可執行檔或未知格式；無檔案帶執行權限；無第三方素材或素材原始壓縮包。
8. **暫定標示**：`region`／`map`／`npc` 的 `name` 含「暫定」；README 與本文件標明世界觀為暫定。
9. **Git 狀態**：`git status --short` 只顯示預期的新增／修改；確認沒有誤加暫存檔或素材。

### 重跑方式

- manifest 雜湊重算（不寫檔，只印出）：
  ```bash
  node -e "const fs=require('fs'),c=require('crypto');for(const p of ['world/maps/starting_village.json']){const b=fs.readFileSync(p);console.log(p,c.createHash('sha256').update(b).digest('hex'),b.length)}"
  ```
  （實際檔案清單請以 `manifest.json` 的 `files` 為準，逐筆重算比對。）
- 完整驗證器由內容工程師在系統暫存區執行（本倉庫不放可執行邏輯）。輸出格式固定為「通過 N，失敗 M」並列出錯誤。若未來要長期保留，建議另立任務放進 `game-client` 的 tools 目錄，不要放進 `game-content`。

## 14. 已知限制（v0.1.0）

- 尚無旗標定義檔、任務、敵人、技能、戰鬥、商店、存檔格式。
- `repeatable: false` 的「只觸發一次」需 client 依 `flag_chest_forest_herb_opened` 判斷；旗標持久化尚未實作。
- `display_key` 由 client 內建 placeholder 對應；本倉庫沒有任何圖像資產。
- 地圖僅 2 張；尚無敵人、NPC 巡邏、商店、任務鏈。
- 本版事件 `kind` 只支援 `dialogue`；戰鬥事件、演出事件尚未定義。
