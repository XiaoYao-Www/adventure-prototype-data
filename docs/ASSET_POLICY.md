# 素材與授權政策

> 狀態：`v0.1.0`。本輪**沒有任何第三方素材**；倉庫內沒有 `assets/` 目錄、沒有圖像或音訊檔案。
> `display_key`（例如 `npc_elder`、`chest`）只是 client 產生 placeholder 的鍵值，不是資產路徑，也不代表任何已授權素材。

## 1. 核心原則

- 可下載不代表可商用，也不代表可再散布。
- 可修改不代表可把原始素材包重新上傳或轉發。
- 若授權禁止再散布，需先確認遊戲內嵌／DLC／內容包發布方式是否允許；不確定就先隔離。
- 每項正式發布資產都必須能追溯至來源與授權證據；授權文件缺失時標示 `license_unverified`，禁止發布。
- 不得移除必要署名或授權聲明。
- 不得把受限素材包當成可自由轉發的獨立素材庫。

## 2. 目前狀態（v0.1.0）

| 項目 | 狀態 |
| --- | --- |
| 第三方素材數量 | 0 |
| `assets/` 目錄 | 不存在（本輪不建立） |
| 圖像／音訊檔案 | 無 |
| placeholder 來源 | 由 `game-client` 以程式產生（色塊、文字或簡單圖形） |
| 素材授權阻塞 | 無（因無素材） |
| 內容包可發布性 | 仍為**暫定**內容；見 `docs/CONTENT_SCHEMA.md` 與 `docs/CONTENT_CHANGELOG.md` |

- 本內容包的 `manifest.json` `files` 只包含 JSON 資料，不含任何資產。
- 若未來要加入資產，必須先擴充 manifest 與 schema（提升 `schema_version`），並完成授權審核。不得直接塞入資產而不更新契約。

## 3. 素材登錄流程（未來啟用）

1. 使用者提供素材來源與完整授權文件（AI 不自行下載、不自行安裝素材）。
2. 在 `asset_catalog/packs/<pack_id>.json` 建立紀錄，欄位依工作區 `TEMPLATES/CONTENT_ASSET_RECORD.md`：
   - `pack_id`、名稱、作者、來源 URL、下載日期、版本／revision、原始壓縮檔 hash。
   - 尺寸、色彩風格、透視、分類（地形／角色／物品／UI）。
   - 授權名稱、授權全文位置、商用／修改／再散布／署名（yes／no／conditional／unknown）。
   - 允許的封裝方式（game bundle／DLC／PCK／internal only／unknown）。
   - 視覺審核與授權審核狀態（pending／approved／rejected）、審核人。
3. 授權全文副本放在 `asset_catalog/licenses/<pack_id>/`（僅在授權允許保存時）。
4. 原始包保持唯讀；轉換輸出到 `generated/` 或正式整理目錄，保留來源映射。
5. 視覺與授權雙審核通過後，才可建立 client 用的資產索引與 `display_key` 對應。
6. 更新 `docs/CONTENT_CHANGELOG.md`，記錄素材來源與授權摘要。

## 4. 授權紅線

- 不得放入來源不明、授權不明或禁止再散布的素材。
- 不得把 CC0、CC-BY、商用許可、可修改與可再散布混為一談。
- 不得提交原始素材壓縮包或受限素材的完整副本。
- 不得因為檔名相似就假設素材屬於同一套。
- 不得在沒有視覺檢查時聲稱風格一致。
- 成人素材必須獨立內容包、預設不啟用，角色明確成年且互動自願。

## 5. `license_unverified` 處理

- 在 `asset_catalog` 紀錄中標記 `license_unverified`，審核狀態不得為 approved。
- 不得列入 `manifest.json` `files`、不得放入 `assets/`、不得進入 `releases/`。
- 不得用於公開發布、DLC 或任何對外散布。
- 解除條件：取得完整授權全文、確認允許的發布方式，並由使用者明確確認。
- 未解除前只能留在本地隔離區（不進 Git）。

## 6. 與 manifest／版本的關係

- 本版 manifest `files` 只列 JSON 資料；未來若納入資產，需先修改契約（`schema_version` 提升），並在 manifest 中明確標示資產類型、大小與雜湊。
- 素材變更不得讓已發布 ID 改變語意；替換素材需保持 client 端索引穩定或提供遷移。
- 發布內容包前必須再次確認所有資產授權仍有效（授權可能隨時間或作者政策改變）。

## 7. 已核對的素材包（2026-11-16）

| pack | 授權 | 商用 | 修改 | 再散布 | 審核狀態 | 依據 |
| --- | --- | --- | --- | --- | --- | --- |
| `mini_world_sprites` | CC0-1.0 | 可 | 可 | 可 | approved | itch.io 頁面 Asset license 欄位＋Information 段 |
| `items16` | 自訂（Glionox） | 可 | 可 | **不可** | approved_with_restriction | itch.io 頁面 Licence 段 |

- 原始檔案不進本倉庫；索引與授權摘要見 `asset_catalog/`。
- `items16` 的 `redistributable=false`：不得把原始 item 圖示放進公開 APK／公開內容包；
  僅限本地與內部測試使用。違反此限制會使內容包不可發布。
- 若作者政策日後改變，以最新來源頁面為準並更新本表與 `asset_catalog/packs/`。
