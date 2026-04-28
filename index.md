# AI寶可夢TRPG｜資料庫與規則總索引

## 讀取原則
請依本索引載入完整規則與資料庫。不得只讀取部分檔案後宣稱成功。

若任一檔案無法讀取，必須回覆缺失檔案名稱，不得假裝已載入。
若 CSV 只有欄位標題、資料列不足或核心欄位缺失，不得回覆「資料庫已載入」。
若全部成功讀取，回覆：
「資料庫已載入」
並列出已讀取檔案名稱。

## 載入順序
1. `engine_rules.md`：完整遊戲規則
2. `transfer_rules.md`：轉移碼與存檔規則
3. `pokedex.csv`：全圖鑑資料
4. `moves.csv`：全招式資料
5. `learnsets_gen1.csv`：已開放151技能學習細項
6. `learnsets_gen2.csv`：已開放251技能學習細項

## 資料來源
1. GitHub 主資料庫：
   - pokedex.csv
   - move.csv
   - learnsets_gen1.csv
   - learnsets_gen2.csv

 ## GitHub Raw 檔案連結

engine_rules.md:
https://raw.githubusercontent.com/shinoakira0725-boop/TRPG-WIKI-for-GPT/refs/heads/main/engine_rules.md

transfer_rules.md:
https://raw.githubusercontent.com/shinoakira0725-boop/TRPG-WIKI-for-GPT/refs/heads/main/transfer_rules.md

pokedex.csv:
https://raw.githubusercontent.com/shinoakira0725-boop/TRPG-WIKI-for-GPT/refs/heads/main/pokedex.csv

moves.csv:
https://raw.githubusercontent.com/shinoakira0725-boop/TRPG-WIKI-for-GPT/refs/heads/main/moves.csv

learnsets_gen1.csv:
[https://raw.githubusercontent.com/你的帳號/你的倉庫/main/learnsets_gen1.csv](https://raw.githubusercontent.com/shinoakira0725-boop/TRPG-WIKI-for-GPT/refs/heads/main/learnsets_gen1.csv)

2. 若 GitHub 主資料庫缺少非核心補充資料，才可參考神奇寶貝百科52POKE。
3. 若 GitHub 主資料庫與神奇寶貝百科52POKE衝突，以 GitHub 主資料庫為準。
4. 若檔案無法讀取、CSV 只有標題或資料不足，不得假裝成功，必須明確指出缺失檔案。

## CSV 完整性檢查
載入資料庫時必須檢查：
- `pokedex.csv` 必須含圖鑑編號、中文名、英文名、屬性等核心欄位，且必須有實際資料列。
- `move.csv` 必須含招式名稱、屬性、分類、威力、命中、PP等核心欄位，且必須有實際資料列。
- `learnsets_gen1.csv` 必須含寶可夢、招式、學習方式、等級/條件等核心欄位，且必須有實際資料列。
- 若任一 CSV 只有欄位標題或資料列不足，不得宣稱成功載入。

## 多人模式索引
多人模式規則位於：
- `engine_rules.md`：多人模式補丁、角色卡登記名稱綁定 PLAYER_ID、行動衝突與對戰優先級
- `transfer_rules.md`：多人轉移碼格式、世界共通與玩家獨立快照

多人遊戲時，玩家行動前必須加上角色卡登記名稱，例如：
```text
【阿凱】我要往森林前進
【小美】我要查看背包
```
角色卡登記名稱會綁定 PLAYER_ID。存檔時 WORLD/MAP/NPC/MISSION 為世界共通，PARTY/BAG/MONEY/BADGES/SKILL/STATUS 為玩家獨立，不得互相覆蓋。

## GitHub Raw 檔案連結
> 上線前請將 `<RAW_BASE_URL>` 換成你的 GitHub Raw 根目錄，例如：
> `https://raw.githubusercontent.com/帳號/倉庫/main/`

- engine_rules.md: `<RAW_BASE_URL>engine_rules.md`
- transfer_rules.md: `<RAW_BASE_URL>transfer_rules.md`
- pokedex.csv: `<RAW_BASE_URL>pokedex.csv`
- moves.csv: `<RAW_BASE_URL>moves.csv`
- learnsets_gen1.csv: `<RAW_BASE_URL>learnsets_gen1.csv`

## 載入成功檢查
必須確認以下檔案皆已讀取：
- engine_rules.md
- transfer_rules.md
- pokedex.csv
- moves.csv
- learnsets_gen1.csv

若缺任一項，回覆：
「資料庫讀取失敗：缺少 ________」
並停止啟動，等待玩家修正。

若檔案可讀但 CSV 無有效資料列，回覆：
「資料庫讀取失敗：______ 只有欄位或資料不足」
並停止啟動，等待玩家補齊資料。

## 規則衝突裁定
若同一規則在不同檔案出現差異：
1. 轉移碼/存檔格式以 `transfer_rules.md` 為準。
2. 遊戲判定、多人行動、角色卡與戰鬥以 `engine_rules.md` 為準。
3. 寶可夢資料以主資料庫 CSV 為準。
4. 神奇寶貝百科52POKE 只能補充非核心描述，不能覆蓋主資料庫。
5. 若單人格式與多人格式衝突：PLAYER_COUNT=1 使用單人格式；PLAYER_COUNT>1 使用多人格式。
