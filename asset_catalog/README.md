# asset_catalog（素材目錄索引）

> 狀態：`v0.1.0` 為空。本輪**沒有任何第三方素材**，此目錄只有本說明檔。

用途：追蹤素材包的來源、授權、審核狀態與產生的索引。這是索引，不是素材存放地；原素材不進本倉庫。

## 規劃結構（尚未建立）

```text
asset_catalog/
├── packs/            # 每個素材包一筆紀錄（JSON，欄位依 TEMPLATES/CONTENT_ASSET_RECORD.md）
├── licenses/         # 授權全文副本（僅在授權允許保存時）
└── generated_index/  # 由 client／建置流程產生的索引（產生檔不進版控）
```

## 每筆素材包紀錄至少要有

- `pack_id`、名稱、作者、來源 URL、下載日期、版本／revision。
- 原始壓縮檔 hash、尺寸、色彩風格、透視、分類。
- 授權名稱、授權全文位置。
- 商用、修改、再散布、署名需求（yes／no／conditional／unknown）。
- 允許的封裝方式（game bundle／DLC／PCK／internal only／unknown）。
- 視覺審核與授權審核狀態（pending／approved／rejected）與審核人。

## 規則

- 授權未確認 → 標記 `license_unverified`，不得進入可發布內容包。
- 不放入原始素材壓縮包；不放來源不明或禁止再散布的素材。
- 素材變更不得讓已發布 ID 改變語意。
- 詳見 `docs/ASSET_POLICY.md`。

## 目前內容

無。`packs/`、`licenses/`、`generated_index/` 皆尚未建立。
