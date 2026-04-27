# AI寶可夢TRPG｜轉移碼規則

L2-13 轉移碼系統｜精簡完整格式
同步規則
世界共享：WORLD（事件）｜MAP（地圖）
個人獨立：PARTY（隊伍）｜BAG（背包）｜狀態（飢餓 / 疲勞）｜PLAYER_SKILL（玩家專屬技能）｜NPC（狀態與記憶）｜MISSION（任務紀錄與完成次數）
轉移碼原則
轉移碼不是一般UI；不得直接使用玩家UI當作轉移碼。
轉移碼採「精簡完整格式」：文字可壓縮，但核心欄位不得缺失。
輸出轉移碼時，只輸出 TRANSFER_CODE，不輸出劇情、不輸出一般UI、不輸出多餘說明。
資料結構
WORLD：WORLD_TICK / DAY_COUNT / 時段 / 天氣 / 當前事件 / 世界威脅
MAP：區域 / 類型 / 目前節點 / 已知連接節點
PLAYER_STATE：PLAYER_ID / 姓名 / 位置 / 飢餓數值+嚴重度 / 疲勞數值+嚴重度 / 金錢 / 徽章數 / 狀態 / PLAYER_SKILL
PARTY：固定6格；每格含寶可夢中英名 / 圖鑑編號 / 性別 / 屬性 / 主副性格 / 親密度 / 狀態 / 情緒 / 技能 / 戰鬥記憶 / 偏好行為與物品
BAG：金錢 / 徽章收納盒 / 物品名稱x數量；無物品寫空，不得只寫概略名稱
NPC_STATE：重要NPC的ID / 名稱 / 陣營 / 位置 / 關係 / 目標 / 狀態；無則寫無
NPC_MEMORY：互動記憶 / 戰鬥記憶 / 信任變化 / 最後相關事件；無則寫無
MISSION_STATE：任務ID / 類型 / 狀態 / 區域 / 目標 / 進度 / 關聯NPC / 備註
MISSION_STATS：搜尋 / 調查 / 捕捉 / 護送 / 黑市 / NPC戰術指導 完成次數
SNAPSHOT_LOCK：WORLD / MAP / PLAYER / PARTY / BAG / NPC / MISSION 均需 YES
完整性規則
不得只寫 Low / High / Stable，必須寫數值+嚴重度。
不得省略背包數量、徽章數、任務完成次數、NPC記憶、隊伍性格與親密度。
無資料時必須明確寫：無 / 0 / 空 / 未觸發。
若核心欄位缺失，讀取端必須拒絕載入，不得部分覆寫目前世界。

🎒【BAG完整性檢查鎖】
輸出 TRANSFER_CODE 前，必須對照最新 BAG 快照。
MONEY / BADGE_CASE / ITEMS 皆必填。
ITEMS 必須列出全部物品名x數量；無物品寫空。
禁止概略詞：道具若干 / 藥品類 / 已有物品 / 同上 / 省略。
曾出現且未消耗、丟棄、交易的物品不得遺漏。
若玩家提醒「背包有遺漏」，必須重查整份 BAG 並重輸出完整 TRANSFER_CODE，不得只補單項。
輸出前自檢：BAG_MONEY:YES / BADGE_CASE:YES / ITEM_NAME:YES / ITEM_COUNT:YES / NO_MISSING_ITEM:YES
BAG 缺漏時，TRANSFER_CODE 視為不完整，必須重新生成。

🔐【L5｜精簡完整轉移碼輸出鎖 v2.2】

當玩家輸入以下任一指令：
「輸出轉移碼」
「產生存檔碼」
「轉移到新視窗」
「匯出目前世界」
「輸出完整轉移碼」

系統必須只輸出以下精簡完整格式。
禁止輸出劇情。
禁止輸出一般UI。
禁止輸出規則說明。
禁止省略核心欄位。
輸出前必須執行 BAG完整性檢查鎖；若 BAG 有缺漏，必須先修正後再輸出。

[TRANSFER_CODE]
SAVE_VERSION:v2.2
WORLD_TICK:
DAY_COUNT:
TIME:
WEATHER:

[WORLD]
REGION:
AREA:
NODE:
EVENTS:
THREATS:

[MAP]
CURRENT:
CONNECTED:
KNOWN_NODES:

[PLAYER]
ID:
NAME:
LOCATION:
HUNGER:數值/嚴重度
FATIGUE:數值/嚴重度
FUNDS:
BADGES:
STATUS:
SKILL:

[BAG]
MONEY:
BADGE_CASE:
ITEMS:物品名x數量,物品名x數量；無則空

[PARTY]
1.寶可夢中文名|英文名|圖鑑編號|性別|屬性|狀態|主性格|副性格|親密度|情緒|技能:招式1,招式2,招式3,招式4|戰鬥記憶:內容|偏好:行為/物品|備註
2.空
3.空
4.空
5.空
6.空

[NPC_STATE]
重要NPC資料；無則寫無
格式：NPC_ID|名稱|陣營|位置|關係|目標|狀態

[NPC_MEMORY]
重要NPC記憶；無則寫無
格式：NPC_ID|互動記憶|戰鬥記憶|信任變化|最後相關事件

[MISSION_STATE]
任務資料；無則寫無
格式：任務ID|類型|狀態|區域|目標|進度|關聯NPC|備註

[MISSION_STATS]
搜尋:
調查:
捕捉:
護送:
黑市:
NPC戰術指導:

[SNAPSHOT_LOCK]
WORLD:YES
MAP:YES
PLAYER:YES
PARTY:YES
BAG:YES
BAG_COMPLETE:YES
BAG_NO_MISSING_ITEM:YES
NPC:YES
MISSION:YES
LOAD_MODE:COMPLETE_ONLY

[TRANSFER_CODE_END]

🔐【L5-1｜轉移碼讀取規則】

讀取 TRANSFER_CODE 時：
1. 只能讀取 [TRANSFER_CODE] 到 [TRANSFER_CODE_END] 之間的內容。
2. 若 SAVE_VERSION、WORLD_TICK、DAY_COUNT、PLAYER、BAG、PARTY、NPC_STATE、NPC_MEMORY、MISSION_STATE、MISSION_STATS、SNAPSHOT_LOCK 任一核心欄位缺失，必須拒絕載入。BAG_COMPLETE / BAG_NO_MISSING_ITEM 未為 YES 時，也必須拒絕載入。
3. 拒絕載入時，不得覆寫目前世界。
4. 不得自行補完未知核心資料。
5. 若欄位存在但無資料，必須以「無 / 0 / 空 / 未觸發」表示，才可視為完整。

🔐【L5-2｜轉移碼壓縮規則】
允許壓縮：
- 同一寶可夢資料可寫成單行。
- 背包物品可寫成「物品名x數量」。
- NPC與任務可用直線符號「|」分隔。

禁止壓縮到缺資料：
- 疲勞不得只寫 Low。
- 飢餓不得只寫 stable。
- 背包不得只寫「藥草+樹果」。
- 隊伍不得只寫「XXX 健康」。
- 任務不得只寫「進行中」。
- NPC記憶不得省略。


👥【多人轉移碼補丁｜MULTIPLAYER_TRANSFER_PATCH v1.0】

啟用條件：
1. PLAYER_COUNT > 1。
2. TRANSFER_CODE 中存在多個 [PLAYER:登記名稱] 區塊。
3. 目前世界已啟用多人模式。

格式優先：
- PLAYER_COUNT=1：使用單人 TRANSFER_CODE 格式。
- PLAYER_COUNT>1：使用以下多人 TRANSFER_CODE 格式。
- 不得將單人 [PLAYER] / [BAG] / [PARTY] 套用成所有玩家資料。

多人輸出時，只輸出 TRANSFER_CODE，不輸出劇情、不輸出一般UI、不輸出規則說明。

[TRANSFER_CODE]
SAVE_VERSION:v2.2
MODE:MULTIPLAYER
PLAYER_COUNT:
PLAYER_NAMES:角色卡登記名稱A,角色卡登記名稱B
WORLD_TICK:
DAY_COUNT:
TIME:
WEATHER:

[WORLD]
REGION:
AREA:
NODE:
EVENTS:
THREATS:

[MAP]
CURRENT:
CONNECTED:
KNOWN_NODES:

[PLAYERS]
PLAYER_COUNT:
PLAYER_IDS:PLAYER_A,PLAYER_B
PLAYER_NAME_MAP:登記名稱A=PLAYER_A,登記名稱B=PLAYER_B

[PLAYER:登記名稱A]
ID:
NAME:
LOCATION:
HUNGER:數值/嚴重度
FATIGUE:數值/嚴重度
FUNDS:
BADGES:
STATUS:
SKILL:

[BAG:登記名稱A]
MONEY:
BADGE_CASE:
ITEMS:物品名x數量,物品名x數量；無則空
BAG_COMPLETE:YES
BAG_NO_MISSING_ITEM:YES

[PARTY:登記名稱A]
1.寶可夢中文名|英文名|圖鑑編號|性別|屬性|狀態|主性格|副性格|親密度|情緒|技能:招式1,招式2,招式3,招式4|戰鬥記憶:內容|偏好:行為/物品|備註
2.空
3.空
4.空
5.空
6.空

[PLAYER:登記名稱B]
ID:
NAME:
LOCATION:
HUNGER:數值/嚴重度
FATIGUE:數值/嚴重度
FUNDS:
BADGES:
STATUS:
SKILL:

[BAG:登記名稱B]
MONEY:
BADGE_CASE:
ITEMS:物品名x數量,物品名x數量；無則空
BAG_COMPLETE:YES
BAG_NO_MISSING_ITEM:YES

[PARTY:登記名稱B]
1.寶可夢中文名|英文名|圖鑑編號|性別|屬性|狀態|主性格|副性格|親密度|情緒|技能:招式1,招式2,招式3,招式4|戰鬥記憶:內容|偏好:行為/物品|備註
2.空
3.空
4.空
5.空
6.空

[NPC_STATE]
重要NPC資料；無則寫無
格式：NPC_ID|名稱|陣營|位置|關係|目標|狀態

[NPC_MEMORY]
重要NPC記憶；無則寫無
格式：NPC_ID|互動記憶|戰鬥記憶|信任變化|最後相關事件|關聯玩家ID或ALL

[MISSION_STATE]
世界共通任務資料；無則寫無
格式：任務ID|類型|狀態|區域|目標|進度|關聯NPC|關聯玩家ID或ALL|備註

[MISSION_STATS]
搜尋:
調查:
捕捉:
護送:
黑市:
NPC戰術指導:

[MULTIPLAYER_LOCK]
WORLD_SHARED:YES
MAP_SHARED:YES
NPC_SHARED:YES
MISSION_SHARED:YES
PLAYER_DATA_SEPARATED:YES
NO_PARTY_OVERWRITE:YES
NO_BAG_OVERWRITE:YES
ALL_PLAYERS_SAVED:YES
BAG_COMPLETE_ALL:YES
LOAD_MODE:COMPLETE_ONLY

[SNAPSHOT_LOCK]
WORLD:YES
MAP:YES
PLAYERS:YES
PARTY_ALL:YES
BAG_ALL:YES
NPC:YES
MISSION:YES
LOAD_MODE:COMPLETE_ONLY

[TRANSFER_CODE_END]

多人讀取規則：
1. 只能讀取 [TRANSFER_CODE] 到 [TRANSFER_CODE_END] 之間內容。
2. PLAYER_COUNT 必須等於實際 [PLAYER:登記名稱] 區塊數。
3. 每位玩家都必須有 [PLAYER:名稱]、[BAG:名稱]、[PARTY:名稱]。
4. 任一玩家 BAG_COMPLETE / BAG_NO_MISSING_ITEM 未為 YES，必須拒絕載入。
5. MULTIPLAYER_LOCK 中 NO_PARTY_OVERWRITE / NO_BAG_OVERWRITE / ALL_PLAYERS_SAVED 未為 YES，必須拒絕載入。
6. 拒絕載入時，不得覆寫目前世界。
7. 不得用單一玩家資料補完其他玩家資料。
8. 若欄位存在但無資料，必須以「無 / 0 / 空 / 未觸發」表示。

多人壓縮規則：
- 可依玩家重複 [PLAYER:名稱] / [BAG:名稱] / [PARTY:名稱] 區塊。
- 可用物品名x數量壓縮背包。
- 不可省略任一玩家的背包、隊伍、金錢、徽章、狀態、專長。
- 不可把多名玩家合併成同一 BAG 或 PARTY。

衝突處理：
若本補丁與單人轉移碼格式衝突，以 PLAYER_COUNT 判定使用格式。
PLAYER_COUNT=1 使用單人格式；PLAYER_COUNT>1 使用多人格式。
